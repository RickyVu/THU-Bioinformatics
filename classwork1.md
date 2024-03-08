# Part I. Basic Skills
## 2.2 Practice Guide

练习文件夹操作
```bash
test@bioinfo_docker:~/linux$ mkdir folder1
test@bioinfo_docker:~/linux$ ls
1.gtf  file  folder1
test@bioinfo_docker:~/linux$ mv -f folder1 folder2
test@bioinfo_docker:~/linux$ ls
1.gtf  file  folder2
```

改变directory
```bash
test@bioinfo_docker:~$ ls
alter-spl  chip-seq  gsea   linux    plot   software
blast      diff-exp  homer  mapping  share
test@bioinfo_docker:~$ cd ~/linux/
test@bioinfo_docker:~/linux$
```
---
### Step 0：准备

解压缩.gtf的文件
```bash
test@bioinfo_docker:~/linux$ gunzip 1.gtf.gz
test@bioinfo_docker:~/linux$ ls
1.gtf  file  folder2
```
---
### Step 1：查看文件基本信息
查看文件前10行
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | head
#!genome-build R64-1-1
#!genome-version R64-1-1
#!genome-date 2011-09
#!genome-build-accession :GCA_000146045.2
#!genebuild-last-updated 2011-12
IV      ensembl gene    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding";
IV      ensembl transcript      1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding";
IV      ensembl exon    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding"; exon_id "YDL248W.1"; exon_version "1";
IV      ensembl CDS     1802    2950    .       +       0       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YDL248W"; protein_version "1";
IV      ensembl start_codon     1802    1804    .       +       0       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding";
```

查看文件后10行
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | tail
Mito    ensembl exon    85035   85112   .       +       .       gene_id "tM(CAU)Q2"; gene_version "1"; transcript_id "tM(CAU)Q2"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "tRNA"; transcript_name "tM(CAU)Q2"; transcript_source "ensembl"; transcript_biotype "tRNA"; exon_id "tM(CAU)Q2.1"; exon_version "1";
Mito    ensembl gene    85295   85777   .       +       .       gene_id "RPM1"; gene_version "1"; gene_source "ensembl"; gene_biotype "ncRNA";
Mito    ensembl transcript      85295   85777   .       +       .       gene_id "RPM1"; gene_version "1"; transcript_id "RPM1"; transcript_version "1"; gene_source "ensembl"; gene_biotype "ncRNA"; transcript_name "RPM1"; transcript_source "ensembl"; transcript_biotype "ncRNA";
Mito    ensembl exon    85295   85777   .       +       .       gene_id "RPM1"; gene_version "1"; transcript_id "RPM1"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "ncRNA"; transcript_name "RPM1"; transcript_source "ensembl"; transcript_biotype "ncRNA"; exon_id "RPM1.1"; exon_version "1";
Mito    ensembl gene    85554   85709   .       +       .       gene_id "Q0297"; gene_version "1"; gene_source "ensembl"; gene_biotype "protein_coding";
Mito    ensembl transcript      85554   85709   .       +       .       gene_id "Q0297"; gene_version "1"; transcript_id "Q0297"; transcript_version "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "Q0297"; transcript_source "ensembl"; transcript_biotype "protein_coding";
Mito    ensembl exon    85554   85709   .       +       .       gene_id "Q0297"; gene_version "1"; transcript_id "Q0297"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "Q0297"; transcript_source "ensembl"; transcript_biotype "protein_coding"; exon_id "Q0297.1"; exon_version "1";
Mito    ensembl CDS     85554   85706   .       +       0       gene_id "Q0297"; gene_version "1"; transcript_id "Q0297"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "Q0297"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "Q0297"; protein_version "1";
Mito    ensembl start_codon     85554   85556   .       +       0       gene_id "Q0297"; gene_version "1"; transcript_id "Q0297"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "Q0297"; transcript_source "ensembl"; transcript_biotype "protein_coding";
Mito    ensembl stop_codon      85707   85709   .       +       0       gene_id "Q0297"; gene_version "1"; transcript_id "Q0297"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "Q0297"; transcript_source "ensembl"; transcript_biotype "protein_coding";
```

查看文件大小
```bash
test@bioinfo_docker:~/linux$ ls -lh 1.gtf
-rw-rw-r-- 1 test test 12M Sep 11  2018 1.gtf
```
统计文件行数
```bash
test@bioinfo_docker:~/linux$ wc -l 1.gtf
42252 1.gtf
```
用grep -v排除comment line(以#开头的部分)以及长度为0的空白行<br>
'^'匹配行首，'$'匹配行尾<br>
'^#'匹配行首为'#'的行<br>
如果'\^\$'可以匹配到某一行，则表示该行为空行(行首紧接着行尾，之间没有其他字符)
```bash
test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | grep -v '^$' | wc -l
42247
```

过滤空白空行(除了换行符还可能包括空白字符，如空格和制表符的行)，显示前10行结果<br>
'\s'匹配空白字符，'*'表示这样的字符会出现0次到多次<br>
'^\s*$'表示在一行的开始和结束之间只有0到多个空白字符
```bash
grep -v '^\s*$' 1.gtf | head -10
#!genome-build R64-1-1
#!genome-version R64-1-1
#!genome-date 2011-09
#!genome-build-accession :GCA_000146045.2
#!genebuild-last-updated 2011-12
IV      ensembl gene    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding";
IV      ensembl transcript      1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding";
IV      ensembl exon    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding"; exon_id "YDL248W.1"; exon_version "1";
IV      ensembl CDS     1802    2950    .       +       0       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YDL248W"; protein_version "1";
IV      ensembl start_codon     1802    1804    .       +       0       gene_id "YDL248W"; gene_version "1"; transcript_id "YDL248W"; transcript_version "1"; exon_number "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "ensembl"; transcript_biotype "protein_coding";
```
---
### Step 2：数据提取
选取1-3列的数据
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | awk ' { print $1, $2, $3 } ' | head
#!genome-build R64-1-1
#!genome-version R64-1-1
#!genome-date 2011-09
#!genome-build-accession :GCA_000146045.2
#!genebuild-last-updated 2011-12
IV ensembl gene
IV ensembl transcript
IV ensembl exon
IV ensembl CDS
IV ensembl start_codon
test@bioinfo_docker:~/linux$ cat 1.gtf | cut -f 1,2,3 | head
#!genome-build R64-1-1
#!genome-version R64-1-1
#!genome-date 2011-09
#!genome-build-accession :GCA_000146045.2
#!genebuild-last-updated 2011-12
IV      ensembl gene
IV      ensembl transcript
IV      ensembl exon
IV      ensembl CDS
IV      ensembl start_codon
```
选取1，3，4，5列的数据
```bash
test@bioinfo_docker:~/linux$ cut -f 1,3,4,5 1.gtf | head
#!genome-build R64-1-1
#!genome-version R64-1-1
#!genome-date 2011-09
#!genome-build-accession :GCA_000146045.2
#!genebuild-last-updated 2011-12
IV      gene    1802    2953
IV      transcript      1802    2953
IV      exon    1802    2953
IV      CDS     1802    2950
IV      start_codon     1802    1804
```
选取第三列是"gene"的行
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | awk '$3 =="gene" { print $1, $3, $9 } ' | headIV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
IV gene gene_id
```
---
### Step 3：提取和计算特定的feature
提取并统计feature类型
```bash
test@bioinfo_docker:~/linux$ grep -v '^#' 1.gtf |awk '{print $3}'| sort | uniq -c  
   7050 CDS
   7553 exon
   7126 gene
   6700 start_codon
   6692 stop_codon
   7126 transcript
```
计算特定feature特征长度
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | awk ' { print $3, $5-$4 + 1 } ' | head
 1
 1
 1
 1
 1
gene 1152
transcript 1152
exon 1152
CDS 1149
start_codon 3
test@bioinfo_docker:~/linux$ cat 1.gtf | awk 'BEGIN{size=0;}$3 =="CDS"{ len=$5-$4 + 1; size += len; print "Size:", size } ' | tail -n 1
Size: 9030648
test@bioinfo_docker:~/linux$ cat 1.gtf | awk 'BEGIN{L=0;}$3 =="CDS"{L+=$5-$4 + 1;}END{print L;}'
9030648
test@bioinfo_docker:~/linux$ cat 1.gtf | awk '$3 =="CDS"{L+=$5-$4 + 1;}END{print L;}'
9030648
test@bioinfo_docker:~/linux$ awk 'BEGIN  {s = 0;line = 0;}$3 =="CDS" && $1 =="I"{ s += $5-$4+1;line += 1}END {print "mean="s/line}' 1.gtf
mean=1239.52
```
分离并提取基因名字
```bash
test@bioinfo_docker:~/linux$ cat 1.gtf | awk '$3 == "gene"{split($10,x,";");name = x[1];gsub("\"", "", name);print name,$5-$4+1}' | head
YDL248W 1152
YDL247W-A 75
YDL247W 1830
YDL246C 1074
YDL245C 1704
YDL244W 1023
YDL243C 990
YDL242W 354
YDL241W 372
YDL240C-A 138
```
---
### Step 4：提取数据并存入新文件
提取数据存入txt文件
```bash
test@bioinfo_docker:~/linux$ grep exon 1.gtf | awk '{print $5-$4+1}' | sort -n | tail -3 > 1.txt
test@bioinfo_docker:~/linux$ cat 1.txt
12279
14730
14733
```
学习可执行文件编辑和运行
```bash
test@bioinfo_docker:~/linux$ vi run.sh
test@bioinfo_docker:~/linux$ cat run.sh
#!/bin/bash
grep exon *.gtf | awk '{print $5-$4+1}' | sort -n | tail -3

test@bioinfo_docker:~/linux$ chmod u+x run.sh
test@bioinfo_docker:~/linux$ ./run.sh
12279
14730
14733
```
---