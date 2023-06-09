# Parallel Transformers Pytorch

We provide the [weights](https://huggingface.co/luffycodes/Parallel-Roberta-Large) for the Parallel Attention and Feedforward design (PAF) for RoBERTa-Large.

To use this model, use the following [paf_modeling_roberta.py](https://github.com/luffycodes/Parallel-Transformers-Pytorch/blob/main/paf_modeling_roberta.py) file.

## Here is how to use this model to get the features of a given text in PyTorch

```python
# use paf_modeling_roberta.py instead of modeling_roberta
from paf_modeling_roberta import RobertaModel
from transformers import RobertaTokenizer

model = RobertaModel.from_pretrained('luffycodes/parallel-roberta-large')
tokenizer = RobertaTokenizer.from_pretrained('roberta-large')
text = "Replace me by any text you'd like."
encoded_input = tokenizer(text, return_tensors='pt')
output = model(**encoded_input)
```

## Efficient GPU implementation
[gpu_paf_modeling_roberta.py](https://github.com/luffycodes/Parallel-Transformers-Pytorch/blob/main/gpu_paf_modeling_roberta.py) provides an efficient gpu implementation of PAF design for pytorch.

It clubs the computation of key, query, value, and first feedforward network sub-layer(intermediate) computation into one.
```
self.kqv_ffn1.weight.data = torch.cat((attention.self.key.weight.data, attention.self.query.weight.data,
                                               attention.self.value.weight.data,
                                               intermediate.dense.weight.data))
```          
However, I could not efficiently optimize the second feedforward network sub-layer computation to run in parallel.

Note that the output of first intermediate sub-layer of FFNs is sparse as shown by [paper](https://arxiv.org/abs/2210.06313).
Hence, one can exploit that sparsity to make second intermediate sub-layer of FFNs faster (if it is not able to run in parallel).

## What is Parallel Attention and Feed-Forward Design?

![pfa (1)](https://github.com/luffycodes/Parallel-Transformers-Pytorch/assets/22951144/e5b76b1c-5fb1-4263-a23b-a61742fe12ae)

*On the left is the standard Series Attention and Feed-Forward Net Design (SAF) for transformers models. On the right is the Parallel Attention and Feed-Forward Net Design (PAF) used in transformer models like PaLM (Chowdhery et al., 2022) and Mesh-Transformers (Wang, 2021)*

## Evaluation results of [PAF-RoBERTa-Large](https://huggingface.co/luffycodes/parallel-roberta-large)

When fine-tuned on downstream tasks, this model achieves the following results:

Glue test results:

| Task | MNLI | QQP  | QNLI | SST-2 | CoLA | STS-B | MRPC | RTE  |
|:----:|:----:|:----:|:----:|:-----:|:----:|:-----:|:----:|:----:|
|      | 89.3 | 91.7 | 94.3 | 96.2  | 64.0 | 91.0  | 90.4 | 80.1 |


## Citation

If you use this work, please cite:
Investigating the Role of Feed-Forward Networks in Transformers Using Parallel Attention and Feed-Forward Net Design:
https://arxiv.org/abs/2305.13297
```
@misc{sonkar2023investigating,
      title={Investigating the Role of Feed-Forward Networks in Transformers Using Parallel Attention and Feed-Forward Net Design}, 
      author={Shashank Sonkar and Richard G. Baraniuk},
      year={2023},
      eprint={2305.13297},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```

