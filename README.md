# [Progressive Differentiable Architecture Search: Bridging the Depth Gap between Search and Evaluation](https://arxiv.org/abs/1904.12760)
by Xin Chen, [Lingxi Xie](http://lingxixie.com/), [Jun Wu](https://see.tongji.edu.cn/info/1153/6850.htm) and [Qi Tian](https://scholar.google.com/citations?user=61b6eYkAAAAJ&hl=zh-CN).

**This code is based on the implementation of  [DARTS](https://github.com/quark0/darts).**

## Highlights of Progressive DARTS

**Searched on CIFAR10, we achieved currently one of, if not only, the best performance on ImageNet (24.4%/7.4%) under the mobile setting!**

**Our approach is much more stable than DARTS, making it easily generalized to (i) deeper network architecture search, (ii) datasets with more classes (like CIFAR100), so it has potentials to be used to more complex vision tasks.**

**The search process in CIFAR10 or CIFAR100 only requires 0.3 GPU-days, *i.e.*, ~7 hours on one single GPU.**

## Introduction

This repository contains the search and evaluation code for our work Progressive DARTS.

It requires only **0.3 GPU-days** (7 hours on a single P100 card) to finish a search progress on CIFAR10 and CIFAR100 datasets,
much faster than DARTS, and achieves higher classification accuracy on both CIFAR and ImageNet datasets (mobole setting).

## Framework
#### Illustration figure
![pipeline](https://github.com/chenxin061/pdarts/blob/master/pipeline2.jpg)


#### Tricks and description of parameters

## Results
#### Results on CIFAR10 and CIFAR100
![Table_CIFAR](https://github.com/chenxin061/pdarts/blob/master/Table1.png)
#### Results on ImageNet
![Table_ImageNet](https://github.com/chenxin061/pdarts/blob/master/Table2.png)
### Searched cells
#### Normal Cell
![C10_normal](https://github.com/chenxin061/pdarts/blob/master/C10_normal.jpg)
#### Reduction Cell
![C10_reduce](https://github.com/chenxin061/pdarts/blob/master/C10_reduce.jpg)

## Usage

To run our code, you need a GPU with at least **16GB memory**, and equip it with PyTorch 0.4 or above versions.

If you have a GPU with smaller memory, say 12GB, you need to use a smaller batch-size in the search stage.
In the current example, using 64 instead of 96 works -- it is a bit slower but does not impact accuracy much.

*We will post more results with a smaller batch size here for your reference.*

#### Run the following command to perform a search progress on CIFAR10.

```
python train_search.py \\
       --tmp_data_dir /path/to/your/data \\
       --save log_path \\
       --add_layers 6 \\
       --add_layers 12 \\
       --dropout_rate 0.1 \\
       --dropout_rate 0.4 \\
       --dropout_rate 0.7 \\
       --note note_of_this_run
Add --cifar100 if search on CIFAR100.
```

It needs ~7 hours on a single P100 GPU, or 12 hours on a single 1080-Ti GPU to finish everything.

For the parameters, please see our paper (we would provided more explanations in this README soon).

#### The evaluation process simply follows that of DARTS.

###### Here is the evaluation on CIFAR10/100:

```
python train_cifar.py \\
       --tmp_data_dir /path/to/your/data \\
       --auxiliary \\
       --save log_path \\
       --note note_of_this_run
Add --cifar100 if evaluating on CIFAR100.
```

###### Here is the evaluation on ImageNet (mobile setting):
```
python train_imagenet.py \\
       --tmp_data_dir /path/to/your/data \\
       --save log_path \\
       --auxiliary \\
       --note note_of_this_run
```
We also provide pre-trained models of the discovered architecture on CIFAR10 and ImageNet.
You can download them from BaiduYun: [C10](https://pan.baidu.com/s/1d9AS-GNWa7EezZD6bQLHYg)(code: vxby) and [ImageNet](https://pan.baidu.com/s/1tAEqI8MgKF1tDbAnVKcreA)(code: u65g).

##### Here is the usage:
```
python test.py \\
       --auxiliary \\
       --model_path /path/to/your/model \\
       --data /path/to/your/data
```
You will get a valid accuracy of 97.58%.

For ImageNet, replace `test.py` with `test_imagenet.py` and you will get a top-1 valid accuracy of 75.6% and a top-5 valid accuracy of 92.6%.

## Reference

If you use our code in your research, please cite our paper accordingly.

Xin Chen, Lingxi Xie, Jun Wu, Qi Tian, Progressive Differentiable Architecture Search: Bridging the Depth Gap between Search and Evaluation, arXiv preprint arXiv:1904.12760, 2019.
