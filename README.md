# [Progressive Differentiable Architecture Search:Bridging the Depth Gap between Search and Evaluation](https://arxiv.org/abs/1904.12760)
by Xin Chen, [Lingxi Xie](http://lingxixie.com/Home.html), [Jun Wu](https://see.tongji.edu.cn/info/1153/6850.htm) and [Qi Tian](https://scholar.google.com/citations?user=61b6eYkAAAAJ&hl=zh-CN).
**This code is based on the original implementation of  [DARTS](https://github.com/quark0/darts). **

## Search:
```
python train_search.py \
       --tmp_data_dir /path/to/your/data \
       --save log_path \
       --add_layers 6 \
       --add_layers 12 \
       --dropout_rate 0.1 \
       --dropout_rate 0.4 \
       --dropout_rate 0.7 \
       --note note_of_this_run
Add --cifar100 if search on CIFAR100.
```
## Evaluate on CIFAR10/100:
```
python train_cifar.py \
       --tmp_data_dir /path/to/your/data \
       --save log_path \
       --note note_of_this_run
Add --cifar100 if evaluating on CIFAR100.
```
## Evaluate on ImageNet:
```
python train_imagenet.py \
       --tmp_data_dir /path/to/your/data \
       --save log_path \
       --note note_of_this_run
```
