---
layout:     post
title:      "pySeqkit-FASTA/Q文件处理工具"
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
# 1. 背景
FASTA和FASTQ是两种常见的生物序列文件格式。在生信分析过程中，我们经常会对这两种序列文件进行转换、统计、切分等操作，以了解数据的情况，便于进行后续分析。有许多工具能够帮助我们来完成这些过程，比如xxxxx，pySeqkit也是其中的一种。  
pySeqkit是多个python脚本组成的工具包，脚本通过调用`FastaReader.py`、`FastqReader.py`这两个模块来处理输入的FASTA/Q文件中的序列信息。如果你用需要涉及到处理FASTA/Q文件的其他操作，可直接调用这两个模块读取FASTA/Q文件种的序列信息，
# 2. 运行环境
Python 2.7和Python3.5均已测试通过
# 3. 安装
* 使用git安装
```commandline
git clone https://github.com/FlyPythons/pySeqkit.git
```
* 直接下载后安装
```commandline
wget https://github.com/FlyPythons/pySeqkit/archive/master.zip
unzip master.zip
```
* 测试安装
```commandline
cd test
python test.py
```
如无任何报错且结果生成正常，表明测试通过
# 4. 使用
## 4.1 FASTA/Q文件序列信息统计
我们经常需要统计FASTA/Q文件中序列的信息，无论是FASTQ文件中总碱基数、序列个数，还是FASTA文件中的Contig N50等等信息。这些信息可通过`fastaStat.py`和`fastqStat.py`来获得，命令如下：
```commandline
fastaStat.py in.fa > in.fa.stat
fastqStat.py in.fq > in.fq.stat
```
在实际情况中，我们会遇到需要合并统计多个FASTA/Q文件的序列信息，比如多批测序Reads的FASTA/Q文件。
```commandline
fastaStat.py 1.fa 2.fa *.fa > in.fa.stat
fastqStat.py 1.fq 2.fq *.fq > in.fq.stat
```
同时可通过‘-c’参数来调整并行进程的数量，多个文件请一定记得用此参数，加速效果明显。
```commandline
fastaStat.py -c 10 1.fa 2.fa *.fa > in.fa.stat
fastqStat.py -c 10 1.fq 2.fq *.fq > in.fq.stat
```
对于多个FASTA/Q文件，也可将文件路径输出到‘input.fofn’，再运行以下命令：
```commandline
fastaStat.py -c 10 -f in.fofn > in.fa.stat
fastqStat.py -c 10 -f in.fofn > in.fq.stat
```
## 4.2 FASTA/Q文件的切分
在实际分析的过程中，由于过大的序列文件可能导致计算资源不够、计算时间过长等问题，我们通常会对序列文件进行切分。  
序列文件切分通常有两种模式：单个文件序列总个数和单个文件序列总长度。可通过以下命令进行切分：
```commandline
fastaSplit.py -m number -n {max number} -o split in.fa
fastqSplit.py -m number -n {max number} -o split in.fq
```
切分后结果会在当前目录的“split”文件夹中，包括切分后的文件“in.\*.fa”以及包含切分文件路径的文件“split_list”。同时‘fastaSplit.py’还会产生包含序列信息的bed文件“in.\*.fa.bed”  
有多个输入文件时，也可以通过统计脚本相同的参数运行脚本。