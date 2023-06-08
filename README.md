# Parallel Transformers Pytorch

We provide the [weights](https://huggingface.co/luffycodes/Parallel-Roberta-Large) for the parallel attention and feedforward design for Roberta-Large.

To use this model, use the following [modeling_roberta.py](https://github.com/luffycodes/Parallel-Transformers-Pytorch/blob/main/paf_modeling_roberta.py) file.

![pfa (1)](https://github.com/luffycodes/Parallel-Transformers-Pytorch/assets/22951144/e5b76b1c-5fb1-4263-a23b-a61742fe12ae)

## Evaluation results

When fine-tuned on downstream tasks, this model achieves the following results:

Glue test results:

| Task | MNLI | QQP  | QNLI | SST-2 | CoLA | STS-B | MRPC | RTE  |
|:----:|:----:|:----:|:----:|:-----:|:----:|:-----:|:----:|:----:|
|      | 89.3 | 91.7 | 94.3 | 96.2  | 64.0 | 91.0  | 90.4 | 80.1 |

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
