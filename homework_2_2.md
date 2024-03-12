# Part I. Basic Skills
## 2.2 Practice Guide
## 课后作业
1. 列出1.gtf文件中 XI 号染色体上的后 10 个 CDS （按照每个CDS终止位置的基因组坐标进行sort）。
    ```bash
    test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | awk '$1="XI";1' | sort -n -k 5 | tail -10
    XI ensembl CDS 1526321 1531708 . + 0 gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YDR545W"; protein_version "1";
    XI ensembl CDS 1526321 1531708 . + 0 gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YDR545W"; protein_version "1";
    XI ensembl exon 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; exon_id "YDR545W.1"; exon_version "1";
    XI ensembl exon 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; exon_id "YDR545W.1"; exon_version "1";
    XI ensembl gene 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding";
    XI ensembl gene 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding";
    XI ensembl stop_codon 1531709 1531711 . + 0 gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding";
    XI ensembl stop_codon 1531709 1531711 . + 0 gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; exon_number "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding";
    XI ensembl transcript 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding";
    XI ensembl transcript 1526321 1531711 . + . gene_id "YDR545W"; gene_version "1"; transcript_id "YDR545W"; transcript_version "1"; gene_name "YRF1-1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YRF1-1"; transcript_source "ensembl"; transcript_biotype "protein_coding";
    ```
2. 统计 IV 号染色体上各类 feature （1.gtf文件的第3列，有些注释文件中还应同时考虑第2列） 的数目，并按升序排列。
    ```bash
    test@bioinfo_docker:~/linux$ grep -v '^#' 1.gtf | awk '$1=="IV" {print $3}' | sort | uniq -c | sort -n -k 1
    853 start_codon
    853 stop_codon
    886 gene
    886 transcript
    895 CDS
    933 exon
   ```
3. 寻找不在 IV 号染色体上的所有负链上的基因中最长的2条 CDS 序列，输出他们的长度。
    ```bash
    grep -v "^#" 1.gtf | awk '$1=="IV" && $7 == "-" {print $5 - $4 + 1}' | sort -n | tail -2
    6438
    6438
    ```
4. 寻找 XV 号染色体上长度最长的5条基因，并输出基因 id 及对应的长度。
    ```bash
    grep -v "^#" 1.gtf | awk '$1=="XV" {split($10, attr, ";"); print attr[1], $5 - $4 + 1}' | sort -n -k 2 | tail -5
    "YOR396W" 5391
    "YOL081W" 9237
    "YOL081W" 9240
    "YOL081W" 9240
    "YOL081W" 9240
    ```
5. 统计1.gtf列数
    ```bash
    test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | awk '{print "columns of size ", split($0, x, "\t") -1 + split($0, x, ";")-1 }' | sort -n -k 1 | uniq -c
    2116 columns of size  11
    5010 columns of size  12
        1 columns of size  15
    2115 columns of size  16
    8472 columns of size  17
    9932 columns of size  18
    4067 columns of size  19
    10534 columns of size  20
    ```
