---
layout:     post
title:      "tRNAscan-SE"
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
# Pre
Infernal 
# 1 安装
```commandline
cd /path/you/buide/
wget http://trna.ucsc.edu/software/trnascan-se-2.0.0.tar.gz
tar xzf trnascan-se-2.0.0.tar.gz
cd trnascan-se-2.0
./configure --prefix /path/you/install
make
make install
ln -s /path/of/Infernal/bin/cmsearch /path/you/install/bin
ln -s /path/of/Infernal/bin/cmscan /path/you/install/bin
```
# 2 USEAGE
```commandline
Basic Options
  -E         : search for eukaryotic tRNAs (default)
  -B         : search for bacterial tRNAs
  -A         : search for archaeal tRNAs
  -M <model> : search for mitochondrial tRNAs
                 options: mammal, vert
  -O         : search for other organellar tRNAs
  -G         : use general tRNA model (cytoslic tRNAs from all 3 domains included)
  -L         : search using the legacy method (tRNAscan, EufindtRNA, and COVE)
                 use with -E, -B, -A, -O, or -G
  -I         : search using Infernal (default)
                 use with -E, -B, -A, -O, or -G
  -o <file>  : save final results in <file>
  -f <file>  : save tRNA secondary structures to <file>
  -m <file>  : save statistics summary for run in <file>
               (speed, # tRNAs found in each part of search, etc)
  -H         : show both primary and secondary structure components to
               covariance model bit scores
  -q         : quiet mode (credits & run option selections suppressed)

  -h         : print full list (long) of available options
```

* 基本用法
```commandline
# 真核生物
tRNAscan-SE -E -o tRNA.result -f tRNA.structure -m tRNA.stat euk.fasta

# 细菌
tRNAscan-SE -B -o tRNA.result -f tRNA.structure -m tRNA.stat bac.fasta

# 古菌
tRNAscan-SE -A -o tRNA.result -f tRNA.structure -m tRNA.stat arc.fasta

# 线粒体
## 哺乳动物
tRNAscan-SE -M mammal -o tRNA.result -f tRNA.structure -m tRNA.stat mamm.fasta
## 绿色植物
tRNAscan-SE -M vert -o tRNA.result -f tRNA.structure -m tRNA.stat vert.fasta

# 其它细胞器
tRNAscan-SE -O -o tRNA.result -f tRNA.structure -m tRNA.stat other.fasta
```

* 选择**Infernal**或**Legacy**进行tRNA搜索
```commandline
# Infernal
tRNAscan-SE -E -I -o tRNA.result -f tRNA.structure -m tRNA.stat test.fasta
## 默认会使用全部可用cpu跑，可通过--thread选择cpu数
tRNAscan-SE -E -I -o tRNA.result -f tRNA.structure -m tRNA.stat --thread 1 test.fasta

# Legacy, 用tRNAscan, EufindtRNA, and COVE进行搜索
tRNAscan-SE -E -L -o tRNA.result -f tRNA.structure -m tRNA.stat test.fasta
```

