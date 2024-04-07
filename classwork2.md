# PART I. BASIC SKILLS
## 3.1. R Basics
## 课堂作业

iris数据集有几列？每列的数据类型是什么?
```R
#!/usr/bin/env Rscript
print(ncol(iris))
print(nrow(iris))
print(sapply(iris, class))
```
```bash
root@bioinfo_docker:/home/test/R_classwork# ./iris_basic.R
[1] 5
[1] 150
Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species
   "numeric"    "numeric"    "numeric"    "numeric"     "factor"
```

按Species列将数据分成3组，分别计算Sepal.Length的均值和标准差，保存为一个csv文件，提供代码和csv文件的内容。
```R
#!/usr/bin/env Rscript
species.sepal = aggregate(Sepal.Length ~ Species, iris, function(x) c(mean=mean(x), sd=sd(x)))
print(species.sepal)
write.csv(species.sepal, "species_sepal.csv")
```
```bash
root@bioinfo_docker:/home/test/R_classwork# ./mean_sd.R
     Species Sepal.Length.mean Sepal.Length.sd
1     setosa         5.0060000       0.3524897
2 versicolor         5.9360000       0.5161711
3  virginica         6.5880000       0.6358796

root@bioinfo_docker:/home/test/R_classwork# cat species_sepal.csv
"","Species","Sepal.Length.mean","Sepal.Length.sd"
"1","setosa",5.0060000,0.3524897
"2","versicolor",5.9360000,0.5161711
"3","virginica",6.5880000,0.6358796
```

对不同Species的Sepal.Width进行One way ANOVA分析，提供代码和输出的结果。
```R
#!/usr/bin/env Rscript
anova = aov(Sepal.Width ~ Species, iris)
summary(anova)
```
```bash
root@bioinfo_docker:/home/test/R_classwork# ./one_way_anova.R
             Df Sum Sq Mean Sq F value Pr(>F)
Species       2  11.35   5.672   49.16 <2e-16 ***
Residuals   147  16.96   0.115
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```
