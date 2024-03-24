# Part III. NGS DATA ANALYSES
## 1. Mapping
## 课后作业

1. 请阐述bowtie中利用了 BWT 的什么性质提高了运算速度？并通过哪些策略优化了对内存的需求？<br>
    Bowtie是一种基于Burrows-Wheeler Transform的快速比对算法。它利用了BWT的以下性质来提高比对速度：

    提高运算速度：
    利用了BWT的性质，允许从索引序列的末尾开始进行匹配搜索，快速定位候选比对位置。具体来说，Bowtie从DNA序列的末尾字符开始，根据BWT索引序列的转换规则，依次向前回溯，直到找到所有可能的比对位置。这种后向搜索的策略可以减少需要考虑的比对位置的数量，从而提高了比对的速度。

    内存优化：
    Bowtie将BWT索引划分为多个块，每个块可以独立加载到内存中。这样可以根据需要动态加载索引块，而不是将整个索引一次性加载到内存中。Bowtie还用了一种压缩算法对BWT索引进行进一步的压缩，以减少索引在内存中的占用空间。


2. 用bowtie将 THA2.fa mapping 到 BowtieIndex/YeastGenome 上，得到 THA2.sam，统计mapping到不同染色体上的reads数量(即统计每条染色体都map上了多少条reads)。

2. 查阅资料，回答以下问题:
    1. 什么是sam/bam文件中的"CIGAR string"? 它包含了什么信息?<br>
    用于描述比对序列和参考序列之间的差异的一种表示方法。CIGAR 字符串由一系列的操作符和操作符所作用的长度组成。操作符包括M（匹配 ，Match），表示比对的碱基在参考序列和比对序列中都存在；I（插入，Insertion），表示比对序列中存在的碱基在参考序列中不存在；D（删除，（Deletion），表示参考序列中存在的碱基在比对序列中不存在等等。
    2. "soft clip"的含义是什么，在CIGAR string中如何表示？<br>
    Soft Clipping（软剪切）指的是在比对序列的起始或末尾部分存在一些未匹配的碱基。这些未匹配的碱基被认为是由于测序错误、测序质量较低或比对序列的部分未完整比对到参考序列导致的。CIGAR用“S”操作符表示。例如：5S10M7S表示读取序列的前5个碱基被软剪切，然后10个碱基完全匹配，最后读取序列的后7个碱基被软剪切。
    3. 什么是reads的mapping quality? 它反映了什么样的信息?<br>
    衡量比对算法对某个读取序列与参考序列的匹配程度的置信度或可靠性的指标。它反映了比对结果的可靠程度以及可能存在的比对误差或不确定性。

    4. 仅根据sam/bam文件的信息，能否推断出read mapping到的区域对应的参考基因组序列? (提示:参考https://samtools.github.io/hts-specs/SAMtags.pdf中对于MD tag的介绍)

4. 软件安装和资源文件的下载也是生物信息学实践中的重要步骤。请自行安装教程中未涉及的bwa软件，从UCSC Genome Browser下载Yeast (S. cerevisiae, sacCer3)基因组序列。使用bwa对Yeast基因组sacCer3.fa建立索引，并利用bwa将THA2.fa，mapping到Yeast参考基因组上，并进一步转化输出得到THA2-bwa.sam文件。