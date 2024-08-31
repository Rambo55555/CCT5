# CCT5: A Code-Change-Oriented Pre-Trained Model
We note that [a recent work](https://arxiv.org/pdf/2208.08100.pdf) from NTU also focuses on pre-trained models for code change. Since these works are conducted at similar time, we missed to discuss this paper in our Related Work part. We are sorry for that and we hope readers could also pay attention to this paper. 
## Getting Started


### Requirements
```
    pytorch=2.0.0;
    torchvision=0.15.1;
    torchaudio;
    datasets==1.16.1;
    transformers==4.21.1;
    tensorboard==2.12.2;
    tree-sitter==0.19.1;
    nltk=3.8.1;
    scipy=1.10.1;
```

Install the above requirements manully or execute the following script:
```bash
bash scripts/setup.sh
```


## Download and preprocess
1. Download the dataset and models:
```bash
bash scripts/download.sh
```
2. Prepare the dataset for pre-training[optional]
```bash
bash scripts/prepare_dataset.sh
```

## Pretrain the model[optional]
```bash
bash scripts/pre-train.sh -g [GPU_ID]
```

## Task 1: Commit Message Generation
```bash
bash scripts/finetune_msggen.sh -g [GPU_ID] -l [cpp/csharp/java/javascript/python/fira]
```
The released checkpoint may performs better than stated in the paper.
If the evaluation during fine-tuning takes too long, you can adjust the "--evaluate_sample_size" parameter. This parameter refers to the number of cases in the validation set during evaluation.

To evaluate the performance of a specific checkpoint, add the flag "-e" followed by the checkpoint path: 

```bash
bash scripts/finetune_msggen.sh -g [GPU_ID] -l [cpp/csharp/java/javascript/python/fira] -e [path_to_model]

conda env config vars set XDG_CACHE_HOME=/data/DataLACP/rambo/.cache
bash scripts/finetune_msggen.sh -g 0 -l fira -c

pkill -f finetune_msg_gen.py
```
Note that if [path_to_model] is blank, this script will automatically evaluate our released checkpoint.

## Task 2: Just-in-Time Comment Update
```bash
bash scripts/finetune_cup.sh -g [GPU_ID]
```
To evaluate a specific checkpoint like in Task 1, add the flag "-e" followed by the checkpoint path.

Additionally, we have released the the output result of CCT5 and baselines, which is stored at `results/CommentUpdate`. Execute the following script and assign the `path_to_result_file` to evaluate its effectiveness:
```
bash scripts/eval_cup_res.sh --filepath [path_to_result_file]
```
## Task 3: Just-in-Time Defect Prediction
### Only semantic features: 
Fine-tune:
```bash
bash scripts/finetune_jitdp_SF.sh -g [GPU_ID]
```
Evaluate:
```bash
bash scripts/finetune_jitdp_SF.sh -g [GPU_ID] -e [path_to_model]
```
### Semantic features + expert features: 

Fine-tune:
```bash
bash scripts/finetune_jitdp_SF_EF.sh -g [GPU_ID]
```
Evaluate:
```bash
bash scripts/finetune_jitdp_SF_EF.sh -g [GPU_ID] -e [path_to_model]
```

## Task 4: Code Change Quality Estimation

Fine-tune:
```bash
bash scripts/finetune_QE.sh -g [GPU_ID]
```
Evaluate:
```bash
bash scripts/finetune_QE.sh -g [GPU_ID] -e [path_to_model]
```

## Task 5: Review Generation
Fine-tune:
```bash
bash scripts/finetune_CodeReview.sh -g [GPU_ID]
```
Evaluate:
```bash
bash scripts/finetune_CodeReview.sh -g [GPU_ID] -e [path_to_model]
```

## Credit
We reused some code from open-source repositories. We would like to extend our gratitude to the following repositories:
1. [CodeT5](https://github.com/salesforce/CodeT5)
2. [CodeBERT](https://github.com/microsoft/CodeBERT)
2. [NatGen](https://github.com/saikat107/NatGen)


## Citation
```
@inproceedings{lin2023cct5,
  title={CCT5: A Code-Change-Oriented Pre-Trained Model},
  author={Lin, Bo and Wang, Shangwen and Liu, Zhongxin and Liu, Yepang and Xia, Xin and Mao, Xiaoguang},
  booktitle={Proceedings of the 31th ACM Joint European Software Engineering Conference and Symposium on the Foundations of Software Engineering},
  year={2023}
}
```

# 结果：
用CodeT5 finetune结果：Test BLEU: 18.84, Test ACC: 1.4882506527415142

用CCT5 finetune结果：Test BLEU: 18.12, Test ACC: 1.3315926892950392
