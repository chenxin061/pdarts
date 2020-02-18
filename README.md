# [Progressive Differentiable Architecture Search: Bridging the Depth Gap between Search and Evaluation](https://arxiv.org/abs/1904.12760)
by Xin Chen, [Lingxi Xie](http://lingxixie.com/), [Jun Wu](https://see.tongji.edu.cn/info/1153/6850.htm) and [Qi Tian](https://scholar.google.com/citations?user=61b6eYkAAAAJ&hl=zh-CN).

**P-DARTS has been accepted for oral presentation at ICCV 2019!**

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
Here we provide the searched cells in one run on CIFAR10 (the paper included those from another run). Note that the searched normal and reduction cells are **deeper** (there are 3 conv layers cascaded one after another) than those searched by DARTS. This shows that our progressive search strategy really found something different, because we considered deeper architectures during search. Please read the paper for a comparison between search in different depths.

![stages](https://github.com/chenxin061/pdarts/blob/master/stages.png)
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
Our test with a limitation of 12 memory on P100 GPU tooks about 9 hours to finish the search. 

For the parameters, please see our paper (we would provided more explanations in this README soon).

#### The evaluation process simply follows that of DARTS.

###### Here is the evaluation on CIFAR10/100:

```
python train_cifar.py \\
       --tmp_data_dir /path/to/your/data \\
       --auxiliary \\
       --cutout \\
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
You can download them from BaiduYun: [C10](https://pan.baidu.com/s/1d9AS-GNWa7EezZD6bQLHYg)(code: vxby) and [ImageNet](https://pan.baidu.com/s/1tAEqI8MgKF1tDbAnVKcreA)(code: u65g) or Google Drive: [C10](https://drive.google.com/open?id=1XsU7Lsh8gyFvIE-mP7LATiHbBr2f_FMH) and [ImageNet](https://drive.google.com/open?id=1w8DiQInnDnUZCYgb49mo9YsLLBibY-gX).

##### Here is the usage:
```
python test.py \\
       --auxiliary \\
       --model_path /path/to/your/model \\
       --data /path/to/your/data
```
You will get a valid accuracy of 97.58% on CIFAR10.

For ImageNet, replace `test.py` with `test_imagenet.py` and you will get a top-1 valid accuracy of 75.6% and a top-5 valid accuracy of 92.6%.

## Reference

If you use our code in your research, please cite our paper accordingly.

Xin Chen, Lingxi Xie, Jun Wu, Qi Tian, Progressive Differentiable Architecture Search: Bridging the Depth Gap between Search and Evaluation, ICCV, 2019.

or in Bibtex format:
```Latex
@inproceedings{chen2019progressive,
  title={Progressive differentiable architecture search: Bridging the depth gap between search and evaluation},
  author={Chen, Xin and Xie, Lingxi and Wu, Jun and Tian, Qi},
  booktitle={Proceedings of the IEEE International Conference on Computer Vision},
  pages={1294--1303},
  year={2019}
}
```

You may also be interested in PC-DARTS, a memory-efficient differentiable architecture search method. Please refer to its [arxiv link](https://arxiv.org/pdf/1907.05737.pdf) and [code](https://github.com/yuhuixu1993/PC-DARTS). 
