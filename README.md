<div align="center">

# 生物信息学
### 清华生物信息学2024春课程资料和代码<br>
[ncRNALab课程](https://www.ncrnalab.org/courses/)<br>
[ncRNALab教学资源](https://book.ncrnalab.org/teaching/)


</div>


---

## 课堂1作业
### Getting Started
1. ✅ 注册一个GitHub账户，创建一个repo(仓库)，写好README.md
2. ✅ 开始定期 Backup your work
3. ✅ 学习使用Markdown语言，熟悉其语法, 利用Github的Github Page功能，用Markdown写一个自己的网页，记录一下：1）自己第一堂课的课堂笔记，尤其要解释一下算法(algorithm)和模型(model)的区别；2）本学期生物信息学的学习计划。（注意内容和格式也是评分的标准，内容上要充实、实用和简洁，不写空话和废话；格式上要利用 Markdown 的格式突出条理和重点）

    **1. 课堂笔记：**
    - 每周课堂以外额外花3-5小时阅读和练习生物信息学
    - Science里面提出了的150条重大科学问题，头5条当中有3条是与生物有关，其中有2条与生物信息学相关
    - 什么是基因？基因是信息（information），而且是Language/Code of Life
    - 人类的基因组大小相对于很多生物小（特别是低等生物），人类有大概3Gb的基因，而阿米巴是达到670Gb
    - 人类基因少是因为可变剪接的现象，一个基因可以通过可变剪接制造多种蛋白质（而且有很多基因是做调控）
    - RNA的发现非常重要，其中最重要的几个是:
        - mRNA
        - Catalytic RNA
        - miRNA (microRNA)
        - LincRNA (Long ncRNA，包括MALAT 1，HOTAIR，...)
    - 为什么有必要使用生物信息？因为随着测序技术的发展，我们的数据变得越来越多，进入了生物大数据的时代
    - 除了数据多，数据还是高纬度的，有大量特征和隐藏特征


    **2. Algorithm和Model的区别：**
    算法是一個問題解決的步驟和規則集（一系列清晰、有序的指令）。而模型則是對問題或系統的抽象表示，基於特定的理論、假設、數學公式或統計方法建立。算法描述了如何處理數據和執行操作，而模型則描述了數據之間的關係和預測結果。在某些情況下，模型可以使用算法來實現，但算法和模型之間並不是一對一的關係，它們描述的是不同層面的問題解決方法

    **3. 学习计划：**
    已经有一定的编程基础，对Python认识比较深，所以打算可以多尝试使用Python来完成作业。Linux CLI，Bash，R以前有学过一点，但是使用得很少，所以也想通过这一次课程学习更多和加深印象。但是最主要还是想学习生物信息学的数据分析和建模部分。计划除了上课以外阅读老师推荐的《Bioinformatics Data Skills》来更快和有效地学习生物信息学基础。

### Setup
1. ✅ 在Windows、MacOS下安装和练习使用Typora, Sublime Text等编辑器
2. ✅ 安装和学习使用Docker (Install Docker, load an image and create a container)。利用 Mac 和 Windows 系统自带的录屏工具录制一个不超过15秒的使用录屏。请用自己的姓名的拼音命名 container，例如 zhangsan_linux
```bash
mkdir bioinfo
cd bioinfo

docker load -i bioinfo_PartI-PartII-PartIII1-3.tar.gz

mkdir bioinfo_tsinghua_share

docker run --name=[CONTAINER_NAME] -dt  -h bioinfo_docker --restart unless-stopped -v %cd%/bioinfo_tsinghua_share:/home/test/share xfliu1995/bioinfo_tsinghua:2

docker exec -it [CONTAINER_NAME] bash
```
3. ✅ 在Docker的Linux Terminal里面打开 Vim，练习完全通过键盘来完成各种编辑操作

### Linux
1. ✅ 对于示例文件（test_command.gtf），尝试使用相关命令或命令组合分别统计文件的行数以及字符数
```bash
cat test_command.gtf | wc -l
# 8
cat test_command.gtf | wc -c
# 636
```
2. ✅ 利用 grep 等命令尝试筛选并输出示例文件中以 chr_ 起始，并且基因id为 YDL248W 的行
```bash
grep '^chr_' test_command.gtf | grep -w 'gene_id "YDL248W"'

# Result
# chr_IV  ensembl gene    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1";
# chr_IV  ensembl transcript      802     2953    .       +       .       gene_id "YDL248W"; gene_version "1";
# chr_IV  ensembl start_codon     1802    1804    .       +       0       gene_id "YDL248W"; gene_version "1";
```
3. ✅ 利用 sed 等命令将示例文件中的 chr_ 替换为 chromosome_ 并输出每行的第1，3，4，5列。（无需改动原文件，只输出结果）
```bash
sed 's/chr_/chromosome_/g' test_command.gtf| cut -f 1,3,4,5

# Result
# chromosome_IV   gene    1802    2953
# chromosome_IV   transcript      802     2953
# chromosome_IV   exon    1802    2953
# chromosome_IV   CDS     1802    950
# chromosome_IV   start_codon     1802    1804
# chromosome_IV   stop_codon      2951    2953
# chromosome_IV   gene    762     3836
# chromosome_IV   transcript      3762    836
```
4. ✅ 通过man命令以及更多的资料学习简单的 awk 命令，尝试互换示例文件的第2列和第3列，并且对输出结果利用 sort 命令依照第4和第5列数字大小排序，将最终结果输出到result.gtf文件中
```bash
awk '{temp = $2; $2 = $3; $3 = temp}1' test_command.gtf | sort -k 4 -k 5 -n  > result.gtf

# Result in result.gtf
# chromosome_IV gene ensembl 762 3836 . + . gene_id "YDL247W-A"; gene_version "1";
# chr_IV transcript ensembl 802 2953 . + . gene_id "YDL248W"; gene_version "1";
# chromosome_IV CDS ensembl 1802 950 . + 0 gene_id "YDL248W"; gene_version "1";
# chr_IV start_codon ensembl 1802 1804 . + 0 gene_id "YDL248W"; gene_version "1";
# chr_IV gene ensembl 1802 2953 . + . gene_id "YDL248W"; gene_version "1";
# chromosome_IV exon ensembl 1802 2953 . + . gene_id "YDL248W"; gene_version "1";
# chromosome_IV stop_codon ensembl 2951 2953 . + 0 gene_id "YDL248W"; gene_version "1";
# chr_IV transcript ensembl 3762 836 . + . gene_id "YDL247W-A"; gene_version "1";
```
5. ✅ 更改示例文件的权限，使得文件所有者及所在用户组用户可读、写、执行而其他用户只可读，展示权限修改前后的权限变化。
```bash
ls -l test_command.gtf
# -rwxrwxrwx 1 test test 636 Mar  2 07:24 test_command.gtf

chmod u=rwx,g=rwx,o=r test_command.gtf
ls -l test_command.gtf
# -rwxrwxr-- 1 test test 636 Mar  2 07:24 test_command.gtf
```

---

## 课堂2作业
[上机作业1](./classwork1.md) <br>
课后作业2:
- [Part I - 2.2 Practice Guide](./homework_2_2.md)
- [Part I - 2.3 Linux Basics](./homework_2_3.md)


## 课堂3作业
课后作业3：
- [Part II - 1. Bash](./homework_3.md)

## 课堂4作业
课后作业4：
- [Part III](./homework_4.md)