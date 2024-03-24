# Part II. Basic Analyses
## 1. Blast
## 课后作业

对于序列:
```MSTRSVSSSSYRRMFGGPGTASRPSSSRSYVTTSTRTYSLGSALRPSTSRSLYASSPGGVYATRSSAVRL```

1) 请使用网页版的 blastp, 将上面的蛋白序列只与 mouse protein database 进行比对， 设置输出结果最多保留10个， E 值最大为 0.5。将操作过程和结果截图，并解释一下 E value和 P value 的实际意义。<br>
操作过程：
![Blast Result](./img/homework3-1.png)
结果：
![Blast Result](./img/homework3-2.png)
    E值（Expect value）是衡量BLAST比对结果的期望统计显著性的指标。它表示在随机情况下，我们预计会在给定的数据库中发现与查询序列具有相同或更高的相似性得分的比对结果的数量。较小的E值表示比对结果更显著，即更不可能由于随机匹配而产生。

    P值（Probability value）是E值的统计显著性指数的一种表示方式。它表示在随机情况下，获得与查询序列具有相同或更高相似性得分的比对结果的概率。较小的P值表示比对结果更显著，即更不可能由于随机匹配而产生。

2) 请使用 Bash 脚本编程：将上面的蛋白序列随机打乱生成10个， 然后对这10个序列两两之间进行 blast 比对，输出并解释结果。（请上传bash脚本，注意做好重要code的注释；同时上传一个结果文件用来示例程序输出的结果以及你对这些结果的解释。）
    ```bash
    #!/bin/bash

    # 原始蛋白序列
    protein_sequence="MSTRSVSSSSYRRMFGGPGTASRPSSSRSYVTTSTRTYSLGSALRPSTSRSLYASSPGGVYATRSSAVRL"

    # 生成10个随机打乱的蛋白序列
    random_sequences=()
    for ((i=1; i<=10; i++))
    do
        random_sequence=$(echo "$protein_sequence" | fold -w1 | shuf | tr -d '\n')
        random_sequences+=("$random_sequence")
    done

    # 对每对序列进行比对
    for ((i=0; i<10; i++))
    do
        for ((j=0; j<10; j++))
        do
            echo "Comparing sequence ${random_sequences[i]} with sequence ${random_sequences[j]}"

            # 在此处使用适当的blastp命令进行比对，并将结果输出为表格
            blastp -query <(echo "${random_sequences[i]}") -subject <(echo "${random_sequences[j]}") -outfmt 6 
            echo
        done
    done
    ```
    **以下的Blastp输出都是以表格输出，列为这些：**
    ```
    # Fields: query acc.ver, subject acc.ver, % identity, alignment length, mismatches, gap opens, q. start, q. end, s. start, s. end, evalue, bit score
    ```
    **也就是：**
    - % Identity（相同性百分比）：查询序列和目标序列在比对区域中相同氨基酸的百分比。它表示两个序列之间的相似程度。
    - Alignment Length（比对长度）：查询序列和目标序列之间的比对长度。它指示两个序列之间比对的位置数（氨基酸数）。
    - Mismatches（错配数）：比对中查询序列和目标序列之间氨基酸不同的位置数。它表示非相同替代的数量。
    - Gap Opens（缺口数）：在比对中引入缺口（插入或删除）的次数。它指示其中一个序列中存在插入或删除的区域。
    - q. Start（查询起始位置）：比对在查询序列中的起始位置。它表示比对在查询序列中开始的位置。
    - q. End（查询结束位置）：比对在查询序列中的结束位置。它表示比对在查询序列中结束的位置。                 
    - s. Start（目标起始位置）：比对在目标序列中的起始位置。它表示比对在目标序列中开始的位置。
    - s. End（目标结束位置）：比对在目标序列中的结束位置。它表示比对在目标序列中结束的位置。
    - E-value（期望值）：期望值（E-value）是一种统计指标，表示在随机情况下，预计具有类似或更好分数的比对数量。较低的 E-value 表示比对更具显著性。
    - Bit Score（比特分数）：比特分数是对比对分数进行归一化和缩放的表示。它考虑了评分系统和数据库大小，方便不同搜索结果的比较。较高的比特分数表示更显著的比对。

    ```
    test@bioinfo_docker:~/blast$ bash random_compare.py
    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG
    Query_1 unnamed 62.500  8       3       0       46      53      52      59      0.31    13.5
    Query_1 unnamed 46.667  15      8       0       12      26      2       16      2.2     11.2

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV
    Query_1 unnamed 42.857  14      8       0       13      26      42      55      1.7     11.5
    Query_1 unnamed 46.154  13      7       0       17      29      4       16      3.5     10.4

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY
    Query_1 unnamed 100.000 5       0       0       66      70      59      63      3.4     10.4

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP
    Query_1 unnamed 44.444  9       5       0       3       11      1       9       3.8     10.4
    Query_1 unnamed 46.667  15      8       0       21      35      38      52      7.3      9.6

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV
    Query_1 unnamed 62.500  8       3       0       12      19      57      64      3.5     10.4
    Query_1 unnamed 38.462  13      8       0       32      44      19      31      5.2     10.0

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR
    Query_1 unnamed 100.000 5       0       0       59      63      62      66      0.36    13.1
    Query_1 unnamed 75.000  8       2       0       34      41      61      68      0.51    12.7

    Comparing sequence GSVAYRVSATRPRFGTSSSRSSSGPLTTSSRYTSSRLVYGGSSGTMTRSYAVPMALYRSRLPSSRSASST with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 34.286  35      18      1       6       40      13      42      1.0     11.9

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV
    Query_1 unnamed 44.828  29      16      0       3       31      42      70      0.005   18.1

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP
    Query_1 unnamed 44.444  18      10      0       13      30      46      63      1.0     11.9
    Query_1 unnamed 50.000  16      8       0       7       22      23      38      1.5     11.5
    Query_1 unnamed 36.364  11      7       0       57      67      45      55      9.7      9.2

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 71.429  7       2       0       7       13      37      43      1.6     11.5
    Query_1 unnamed 50.000  6       3       0       51      56      20      25      4.1     10.4

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR

    Comparing sequence TPRTGGARPSSSSATLSMGVYSRRRSYSSGATPLSSTSSSSYSVLSVFRYRLTRSYRGPSRVMAASSTTG with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 63.636  11      4       0       3       13      13      23      0.023   16.2

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY
    Query_1 unnamed 66.667  12      4       0       14      25      35      46      0.19    13.9

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP
    Query_1 unnamed 53.846  13      5       1       51      62      39      51      2.2     11.2

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV
    Query_1 unnamed 50.000  16      7       1       19      33      8       23      4.6     10.0

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV
    Query_1 unnamed 41.176  17      9       1       20      36      4       19      4.4     10.0

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 50.000  20      8       1       14      31      24      43      2.0     11.2

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR
    Query_1 unnamed 77.778  9       2       0       28      36      55      63      0.12    14.2
    Query_1 unnamed 58.824  17      6       1       4       19      5       21      0.40    13.1
    Query_1 unnamed 50.000  4       2       0       54      57      30      33      10.0     9.2

    Comparing sequence RYVSTGRASAGYSTTARSSSVSRSLTPRSASTPYSRTSFVMRLGSTRPLGSSGSLARPSYSSMTRYSSGV with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 60.000  10      4       0       27      36      19      28      0.91    11.9

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP
    Query_1 unnamed 83.333  6       1       0       24      29      18      23      1.1     11.9

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV
    Query_1 unnamed 41.667  12      7       0       5       16      33      44      2.9     10.8

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 38.462  26      6       1       30      45      28      53      3.4     10.4

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR

    Comparing sequence TPPSGTVRMSSLTSTVPAVYSPRSYSSRASSSMVTTAYLSSLSRGLSRGRGYSRGGRTSASSTRASRFSY with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 41.667  24      14      0       22      45      19      42      0.56    12.7

    Comparing sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP with sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV
    Query_1 unnamed 50.000  26      10      1       1       26      15      37      0.11    14.6

    Comparing sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV

    Comparing sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 37.838  37      23      0       10      46      5       41      0.035   15.8

    Comparing sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR
    Query_1 unnamed 44.444  9       5       0       16      24      47      55      2.8     10.8

    Comparing sequence LPYGASSTRRYSGSVSASYGSRARRTSSSFLLMRSTYSSASSLPRSPTYTSAVSTMRRSRSTGTGSVGVP with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 60.000  10      4       0       10      19      57      66      0.24    13.5

    Comparing sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV with sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV

    Comparing sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 47.368  19      10      0       17      35      7       25      0.40    13.1

    Comparing sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR

    Comparing sequence TTTGFARSSYRRSVVPASYSSAPRLRGSVSGSRAYRGSTSGLSTSSRYLSSYRSPSPRTLTSSTSMMGAV with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 46.667  15      8       0       24      38      29      43      0.55    12.7

    Comparing sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV with sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA
    Query_1 unnamed 37.838  37      16      2       17      53      9       38      3.9     10.4

    Comparing sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR

    Comparing sequence YYFSMGRTVLTRRATPYRLSSSSSGAARSSRPGSASMSSSVTTLLTYSGRTARRGGPSSSTSSYRVSPSV with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 85.714  7       1       0       30      36      17      23      0.53    12.7
    Query_1 unnamed 55.556  9       4       0       16      24      27      35      1.8     11.2

    Comparing sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA with sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR
    Query_1 unnamed 60.000  10      4       0       49      58      23      32      0.10    14.6
    Query_1 unnamed 57.143  14      5       1       31      44      47      59      2.7     10.8

    Comparing sequence TPVSRYAGYPSSSTTGGSVRTSRTYLRSSSTFTGRYVRPRSSSLYGMSSLARGPTAMSSSVLRSSSSARA with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 80.000  5       1       0       5       9       57      61      0.27    13.5
    Query_1 unnamed 47.368  19      9       1       35      53      25      42      0.33    13.1
    Query_1 unnamed 44.444  9       5       0       2       10      19      27      5.5     10.0

    Comparing sequence TSVPSTGRTTSSGYSALSASSRSTVRSPTAMARLRVSMGYSYSSSSTFTYGYRLRSASVPSSRLPSGGRR with sequence PMYSVTTPGRSARTSGSRPTSASSRYPRRLSTTFSTSRVARGASRMSSVSGSSLSSRYGGYVLTSYLSAS
    Query_1 unnamed 41.026  39      22      1       2       39      4       42      0.37    13.1
    ```
    **拿第一个比对做分析：**<br>

    |           |匹配子序列1 |匹配子序列2  |
    |-----------|----------|-------------|
    |% Identity |62.500%   |46.667%      |
    |比对长度    |8         |15           |
    | 错配数     | 3        |  8          |
    | 缺口数     | 0        | 0           |
    |查询起始位置 |46       |12           |
    |查询结束位置 |53       |26           |
    |目标起始位置 |52       |2            |
    |目标结束位置 |59       |16           |
    |E-value     |0.31     |2.2          |
    |比特分数     |13.5     |11.2         |

    分析和解释：

    匹配子序列1是查询序列位置46-53与目标的52-59匹配。匹配子序列2是查询序列位置12-26与目标的2-16匹配。

    匹配子序列1有更加高的相似度，可以从%Identity，E-value和比特分数看得出。但是子序列2有更加大的重叠区域（14>7）。在两个子序列中，都没有缺口（0），表示没有插入或删除。子序列1有3个匹配错误，子序列2有8个匹配错误，因此导致刚刚提过的子序列1%Identity比子序列2的高。

    总体来说，这次分析意味着原始蛋白质序列与其随机排序版本仍然保留了一定程度的相似性，但由于随机排序，相似性程度有所降低。

3) 解释blast 中除了动态规划（dynamic programming）还利用了什么方法来提高速度，为什么可以提高速度。
    1. 剪枝（Pruning）：BLAST使用了剪枝技术来减少搜索空间并排除不太可能的候选序列。通过设置一些阈值来筛选掉低置信度的序列匹配，BLAST可以减少计算量并提高搜索速度。

    2. 字符串哈希（String Hashing）：BLAST使用了哈希函数来对查询序列和数据库序列进行编码。这样可以将序列比对的过程转换为对哈希表的查找，从而加快了搜索速度。

    3. 预计算（Precomputation）：在BLAST中，一些计算结果可以进行预先计算并保存，以便在后续查询时直接使用。例如，BLAST会对数据库序列进行预处理，计算并存储其位置特征，以便在查询时快速定位匹配。

    4. 部分匹配（Seed）：BLAST利用部分匹配的策略来加速搜索。它首先找到查询序列中的短片段（称为seed），然后在数据库中搜索具有相同或相似seed的序列。这种方法可以大大减少搜索空间，提高搜索速度。

    5. 贪心算法（Greedy Algorithm）：BLAST使用贪心算法来寻找局部最优解。它从最高得分的匹配开始，然后通过不断延伸和扩展来构建局部比对。这种方法可以在不考虑全局最优解的情况下快速找到相对较好的比对结果。

4) 我们常见的PAM250有如下图所示的两种（一种对称、一种不对称），请阅读一下 "Symmetry of the PAM matrices" @ wikipedia，再利用Google/wikipedia等工具查阅更多资料，然后总结和解释一下这两种（对称和不对称）PAM250不一样的原因及其在应用上的不同。
    - 原因：对称的PAM250矩阵假设无论替代的是哪种氨基酸，替代的概率都是相同。而非对称的PAM250矩阵考虑了氨基酸替代的方向性。它认识到从氨基酸A替代为氨基酸B的概率可能与相反的替代概率不同。这个矩阵能够捕捉到氨基酸突变频率的不对称性。

    - 应用：对称和非对称的PAM250矩阵的不同用途在于序列比对任务的具体要求。对称矩阵通常用于全局序列比对方法中，其中整个序列被比对以找到最佳的整体匹配。当替代的方向性不是一个重要因素时，它是合适的选择。非对称矩阵通常用于局部序列比对方法，例如Smith-Waterman算法。局部比对专注于找到序列之间的局部相似区域。非对称矩阵可以捕捉到更细微的替代概率差异，更适用于替代方向性在比对中起作用的情况。