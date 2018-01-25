---
layout:     post
title:      "pySeqkit-实用的FASTA/Q处理工具"
subtitle:   ""
date:       2018-01-16
author:     "FlyPythons"
header-img: ""
catalog: true
tags:
    - Tools
    - Bioinformation
    - Sequence analysis
---
FASTA和FASTQ是两种常见的生物序列文件格式，在生信分析过程中，我们经常会对这两种序列文件进行转换、统计、切分等操作，以了解数据的情况，便于进行后续分析。常用的处理软件包括：
1. seqkit  
2. seqtk

这些工具相当强大，能支持各种操作，但是在对FASTA/Q的数据统计方面稍显薄弱，尤其是在多个文件合并统计中。测序的原始数据通常由多个FASTA/Q文件组成，比如：  
Sequel下机数据bam2fasta后的数据
```commandline
m54061_170922_182836.subreads.fasta
m54136_171108_001745.subreads.fasta
m54143_170814_064401.subreads.fasta
m54152_170801_115501.subreads.fasta
m54139_170821_122203.subreads.fasta
......
```
Nanopore下机fast5转fastq后的数据
```commandline
20171125_NPL0001_A1.fastq
20171125_NPL0001_A2.fastq
20171125_NPL0001_A3.fastq
......
```
由于上述工具简单地完成统计工作，因此，我写了一个工具包[pySeqkit](https://github.com/FlyPythons/pySeqkit)，来完成统计工作，同时工具包中也包含了切分的工具。  

# 1. 介绍
## 基本信息
* 工具类型： 命令行
* 支持格式： fasta, fasta.gz, fastq, fastq.gz
* 编程语言： python
* 支持平台： linux， windows
* 开源地址： https://github.com/FlyPythons/pySeqkit 
* 运行环境： python2.7及以上, python3.5及以上

## 特点
* 统计结果详细
* 支持gzip输入
* 多个文件输入支持并行处理

# 2. 功能
目前为止，pySeqkit工具中有4个工具,分为以下两类：
* 序列统计  
`fastaStat.py` 统计fasta序列的信息  
`fastqStat.py` 统计fastq序列的信息  
* 序列切分  
`fastaSplit.py` 切分fasta序列  
`fastqSplit.py` 切分fastq序列

更多功能正在解锁中......

# 3. 使用
## 序列统计
* 单个文件  
```
fastaStat.py in.fa > in.fa.stat
fastqStat.py in.fq > in.fq.stat
```
* 多个文件
```commandline
fastaStat.py -c 10 1.fa 2.fa *.fa > in.fa.stat
fastqStat.py -c 10 1.fq 2.fq *.fq > in.fq.stat
```
`-c`参数表示同时并行的进程数，默认为1.

程序最终运行结果如下:
```commandline
Statistics for all fasta reads

contig number:          3
sum of contig length:   30
contig average length:  10
longest contig length:  10

Distribution of contig length
 Type             Bases           Count     %Bases
  N10                10               1      33.33
  N20                10               1      33.33
  N30                10               1      33.33
  N40                10               2      66.67
  N50                10               2      66.67
  N60                10               2      66.67
  N70                10               3     100.00
  N80                10               3     100.00
  N90                10               3     100.00
 >1kb                 0               0       0.00
 >5kb                 0               0       0.00
>10kb                 0               0       0.00
>20kb                 0               0       0.00
>30kb                 0               0       0.00
>40kb                 0               0       0.00
>50kb                 0               0       0.00
>60kb                 0               0       0.00
```
\* 对于二代测序产生的短READs的FASTQ文件，加入`-ngs`参数跳过N50等的统计,防止无意义的计算
```commandline
fastqStat.py -ngs -c 10 *.R1.fq *.R2.fq > in.fq.stat
```
## 序列切分
在实际分析的过程中，由于过大的序列文件可能导致计算资源不够、计算时间过长等问题，我们通常会对序列文件进行切分。  
序列文件切分通常有两种模式：
* {i}个序列放到单个文件中
```commandline
fastaSplit.py -m number -n {i} in.fa
fastqSplit.py -m number -n {i} in.fq
```

* 单个文件序列总长不超过{i}
```commandline
fastaSplit.py -m length -n {i} in.fa
fastqSplit.py -m length -n {i} in.fq
```

# 4. 更多功能
该工具包目前功能有限，有以下功能待解锁：
* 个性化定制统计结果  
添加`-N`参数定义需要统计的Nxx结果  
添加`-L`参数定义需要统计的长度阈值结果  
添加绘图功能
* fastq转fasta
 
更多意见可发送邮件至jpfan(at)whu(dot)edu(dot)cn