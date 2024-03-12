# Part I. Basic Skills
## 2.2 Practice Guide
## 课后作业
1. 列出1.gtf文件中 XI 号染色体上的后 10 个 CDS （按照每个CDS终止位置的基因组坐标进行sort）。
    ```bash
    test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | awk '$1=="XI" && $3=="CDS"{print}' | sort -n -k 5 | tail -10
    XI      ensembl CDS     631152  632798  .       +       0       gene_id "YKR097W"; gene_version "1"; transcript_id "YKR097W"; transcript_version "1"; exon_number "1"; gene_name "PCK1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "PCK1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR097W"; protein_version "1";
    XI      ensembl CDS     633029  635179  .       -       0       gene_id "YKR098C"; gene_version "1"; transcript_id "YKR098C"; transcript_version "1"; exon_number "1"; gene_name "UBP11"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "UBP11"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR098C"; protein_version "1";
    XI      ensembl CDS     635851  638283  .       +       0       gene_id "YKR099W"; gene_version "1"; transcript_id "YKR099W"; transcript_version "1"; exon_number "1"; gene_name "BAS1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "BAS1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR099W"; protein_version "1";
    XI      ensembl CDS     638904  639968  .       -       0       gene_id "YKR100C"; gene_version "1"; transcript_id "YKR100C"; transcript_version "1"; exon_number "1"; gene_name "SKG1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "SKG1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR100C"; protein_version "1";
    XI      ensembl CDS     640540  642501  .       +       0       gene_id "YKR101W"; gene_version "1"; transcript_id "YKR101W"; transcript_version "1"; exon_number "1"; gene_name "SIR1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "SIR1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR101W"; protein_version "1";
    XI      ensembl CDS     646356  649862  .       +       0       gene_id "YKR102W"; gene_version "1"; transcript_id "YKR102W"; transcript_version "1"; exon_number "1"; gene_name "FLO10"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "FLO10"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR102W"; protein_version "1";
    XI      ensembl CDS     653080  656733  .       +       0       gene_id "YKR103W"; gene_version "1"; transcript_id "YKR103W"; transcript_version "1"; exon_number "1"; gene_name "NFT1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "NFT1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR103W"; protein_version "1";
    XI      ensembl CDS     656836  657753  .       +       0       gene_id "YKR104W"; gene_version "1"; transcript_id "YKR104W"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YKR104W"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR104W"; protein_version "1";
    XI      ensembl CDS     658719  660464  .       -       0       gene_id "YKR105C"; gene_version "1"; transcript_id "YKR105C"; transcript_version "1"; exon_number "1"; gene_name "VBA5"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "VBA5"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR105C"; protein_version "1";
    XI      ensembl CDS     661442  663286  .       +       0       gene_id "YKR106W"; gene_version "1"; transcript_id "YKR106W"; transcript_version "1"; exon_number "1"; gene_name "GEX2"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "GEX2"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKR106W"; protein_version "1";
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
    test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | awk '$1!="IV" && $3=="CDS" && $7 == "-" {print $5 - $4 + 1}' | sort -n | tail -2
    12276
    14730
    ```
4. 寻找 XV 号染色体上长度最长的5条基因，并输出基因 id 及对应的长度。
    ```bash
    test@bioinfo_docker:~/linux$ grep -v "^#" 1.gtf | awk '$1=="XV" && $3=="gene" {split($10, attr, ";"); print attr[1], $5 - $4 + 1}' | sort -n -k 2 | tail -5
    "YOR142W-B" 5269
    "YOR192C-B" 5314
    "YOR343W-B" 5314
    "YOR396W" 5391
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
