# Appendix VI. Genome Annotation
## 课后作业

请大家查阅网络资源（如NCBI和ENSEMBL）以及文献等资料回答以下问题:
1) 人类基因组的大小以及基本组成是哪些？
2) 基因中的非编码RNA的最新注释是多少个了？请详细列一下其中的非编码RNA 的细分类型的数目，并对主要的非编码RNA 是什么做的用1-2句解释一下。
<br>*注：*
    1. *要注明数字的来源出处和版本更新时间。*
    2. *推荐用markdown语言来写，注意截图显示。*

1. 
    根据NCBI的资料：<br>
    Genome size: 3.1 Gb<br>
    GC percent: 41<br>
    Total Genes: 59,652
    - protein-coding: 20,080
    - non-coding: 22,102
    - Transcribed pseudogenes: 1,225
    - Non-transcribed pseudogenes: 15,772
    - genes with variants: 20,229
    - Immunoglobulin/T-cell receptor gene segments: 400	
    - other: 73	

2. 
    non-coding RNAs: 49,182
    - misc_RNA: 12,182	
    - tRNA: 691
    - rRNA: 55	
    - ncRNA 

    misc_RNA: 指一类在功能上没有明确定义的RNA（核糖核酸）序列。它是一个通用术语，用于分类已经被鉴定但尚未被赋予特定功能或归类为已知RNA类别的RNA分子。<br>
    tRNA: 主要功能是将氨基酸转运到正在合成蛋白质的核糖体。这是通过tRNA分子上的特定序列，即抗密码子，与mRNA分子上的密码子相互配对来实现。<br>
    rRNA: 构成细胞核糖体的主要组成部分。核糖体是细胞中负责蛋白质合成的细胞器。rRNA分子通过融合与蛋白质形成核糖体的结构。

GCF_000001405.40-RS_2023_10:<br>
Date of submission of annotation to the public databases: Oct 6 2023<br>
Software version: 10.2<br>
Reference: https://www.ncbi.nlm.nih.gov/genome/annotation_euk/Homo_sapiens/GCF_000001405.40-RS_2023_10/



# Part III. NGS DATA ANALYSES
## 1.2 bedtools and samtools
## 课后作业

（1）我们提供的bam文件COAD.ACTB.bam是单端测序分析的结果还是双端测序分析的结果？为什么？(提示：可以使用samtools flagstat）<br><br>
是单端测序，因为"paired in sequencing"是0
```bash
test@bioinfo_docker:~$ samtools flagstat COAD.ACTB.bam
185650 + 0 in total (QC-passed reads + QC-failed reads)
4923 + 0 secondary
0 + 0 supplementary
0 + 0 duplicates
185650 + 0 mapped (100.00% : N/A)
0 + 0 paired in sequencing
0 + 0 read1
0 + 0 read2
0 + 0 properly paired (N/A : N/A)
0 + 0 with itself and mate mapped
0 + 0 singletons (N/A : N/A)
0 + 0 with mate mapped to a different chr
0 + 0 with mate mapped to a different chr (mapQ>=5)
```
（2）查阅资料回答什么叫做"secondary alignment"？并统计提供的bam文件中，有多少条记录属于"secondary alignment?" （提示：可以使用samtools view -f 获得对应secondary alignment的records进行统计）<br><br>
"secondary alignment"（次优比对）表示将一条read（DNA或RNA序列）与参考序列进行比对时，除了主要比对之外，还存在其他的备选比对位置。同样flagstat也有显示secondary alignment的数量，COAD.ACTB.bam有4923个secondary alignment。
```bash
test@bioinfo_docker:~$ samtools flagstat COAD.ACTB.bam
185650 + 0 in total (QC-passed reads + QC-failed reads)
4923 + 0 secondary
```
（3）请根据hg38.ACTB.gff计算出在ACTB基因的每一条转录本中都被注释成intron的区域，以bed格式输出。并提取COAD.ACTB.bam中比对到ACTB基因intron区域的bam信息，后将bam转换为fastq文件。<br>
```bash
#会使用gene-exon的方法找到intron，所以首先筛选出hg38.ACTB.gff的gene和exon
test@bioinfo_docker:~$ awk '$3=="gene"{print $0}' hg38.ACTB.gff > hg38.ATCB.gene.gff
test@bioinfo_docker:~$ awk '$3=="exon"{print $0}' hg38.ACTB.gff > hg38.ATCB.exon.gff
```
```bash
#会使用bedops工具来把gff文件转换为bed文件
root@bioinfo_docker:/home/test# git clone https://github.com/bedops/bedops.git
root@bioinfo_docker:/home/test# cd bedops
root@bioinfo_docker:/home/test# make
troot@bioinfo_docker:/home/test# make install
```
```bash
#把gene和exon的.gff转换为.bed
test@bioinfo_docker:~$ gff2bed <hg38.ATCB.gene.gff > hg38.ATCB.gene.bed
test@bioinfo_docker:~$ gff2bed <hg38.ATCB.exon.gff > hg38.ATCB.exon.bed
#计算出intron
test@bioinfo_docker:~$ bedtools subtract -a hg38.ATCB.gene.bed -b hg38.ATCB.exon.bed > hg38.ATCB.intron.bed
```
```bash
#提取COAD.ACTB.bam中比对到ACTB基因intron区域的bam信息
test@bioinfo_docker:~$ bedtools intersect -abam COAD.ACTB.bam -b hg38.ATCB.intron.bed > result.bam
test@bioinfo_docker:~$ bedtools intersect -a hg38.ATCB.intron.bed -b COAD.ACTB.bam -c > result.txt #可直接读文件内容
test@bioinfo_docker:~$ cat result.txt
chr7    5528185 5528280 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    10886
chr7    5529982 5530523 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    5101
chr7    5530627 5540675 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    268
chr7    5540771 5561851 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    290
chr7    5561949 5562389 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    177
chr7    5562828 5563713 ENSG00000075624.17      .       -       HAVANA  gene    .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12    173
```
```bash
#将bam转换为fastq文件
test@bioinfo_docker:~$ samtools sort -n result.sam > result.sort.sam
test@bioinfo_docker:~$ bedtools bamtofastq -i result.sort.sam -fq result.sort.fastq
```
(4) 利用COAD.ACTB.bam计算出reads在ACTB基因对应的genomic interval上的coverage，以bedgraph格式输出。 （提示：对于真核生物转录组测序向基因组mapping得到的bam文件，bedtools genomecov有必要加-split参数。）
```bash
test@bioinfo_docker:~$ samtools sort COAD.ACTB.bam > COAD.ACTB.sort.bam
test@bioinfo_docker:~$ samtools index COAD.ACTB.sort.bam
test@bioinfo_docker:~$ bedtools genomecov -ibam COAD.ACTB.sort.bam -bg -split > COAD.ACTB.coverage.bedgraph
```