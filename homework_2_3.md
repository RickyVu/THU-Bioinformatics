# Part I. Basic Skills
## 2.3 Linux Bash

解压缩bash_homework.zip到linux/bash_homework
```bash
test@bioinfo_docker:~/share$ ls
bash_homework.zip  result.gtf  test_command.gtf
test@bioinfo_docker:~/share$ mv bash_homework.zip ../linux/bash_homework.zip
test@bioinfo_docker:~/share$ cd ..
test@bioinfo_docker:~$ cd linux/
test@bioinfo_docker:~/linux$ ls
1.gtf  1.txt  bash_homework.zip  file  folder2  run.sh
test@bioinfo_docker:~/linux$ unzip bash_homework.zip -d bash_homework
test@bioinfo_docker:~/linux$ ls
1.gtf  1.txt  bash_homework  bash_homework.zip  file  folder2  run.sh
```
编写save_child.sh
```bash
test@bioinfo_docker:~/linux$ vim save_child.sh
```

save_child.sh内容：
```bash
#!/bin/bash

path=$1
DIR=$(tree -R -aif "$path")
file_txt="filename.txt"
dir_txt="dirname.txt"

for val in $DIR
do
  if [ -f $val ]; then
    echo $val >> $file_txt
  elif [ -d $val ]; then
    echo $val >> $dir_txt
  fi
done

exit 0
```

运行save_child.sh，生成filename.txt和dirname.txt
```bash
test@bioinfo_docker:~/linux$ ls
1.gtf  1.txt  bash_homework  bash_homework.zip  file  folder2  run.sh  save_child.sh
test@bioinfo_docker:~/linux$ bash save_child.sh bash_homework/
test@bioinfo_docker:~/linux$ ls
1.gtf  1.txt  bash_homework  bash_homework.zip  dirname.txt  file  filename.txt  folder2  run.sh  save_child.sh
```

展示dirname.txt内容
```bash
test@bioinfo_docker:~/linux$ cat dirname.txt
bash_homework
bash_homework/bash_homework
bash_homework/bash_homework/a-docker
bash_homework/bash_homework/app
bash_homework/bash_homework/backup
bash_homework/bash_homework/bin
bash_homework/bash_homework/biosoft
bash_homework/bash_homework/c1-RBPanno
bash_homework/bash_homework/datatable
bash_homework/bash_homework/db
bash_homework/bash_homework/download
bash_homework/bash_homework/e-annotation
bash_homework/bash_homework/exRNA
bash_homework/bash_homework/genome
bash_homework/bash_homework/git
bash_homework/bash_homework/highcharts
bash_homework/bash_homework/home
bash_homework/bash_homework/hub29
bash_homework/bash_homework/ibme
bash_homework/bash_homework/l-lwl
bash_homework/bash_homework/map2
bash_homework/bash_homework/mljs
bash_homework/bash_homework/module
bash_homework/bash_homework/mogproject
bash_homework/bash_homework/node_modules
bash_homework/bash_homework/perl5
bash_homework/bash_homework/postar2
bash_homework/bash_homework/postar_app
bash_homework/bash_homework/postar.docker
bash_homework/bash_homework/RBP_map
bash_homework/bash_homework/rout
bash_homework/bash_homework/script
bash_homework/bash_homework/script_backup
bash_homework/bash_homework/software
bash_homework/bash_homework/tcga
bash_homework/bash_homework/test
bash_homework/bash_homework/tmp
bash_homework/bash_homework/tmp_script
bash_homework/bash_homework/var
bash_homework/bash_homework/x-rbp
bash_homework/__MACOSX
bash_homework/__MACOSX/bash_homework
```

展示filename.txt内容
```bash
test@bioinfo_docker:~/linux$ cat filename.txt
bash_homework/bash_homework/a1.txt
bash_homework/bash_homework/a.txt
bash_homework/bash_homework/b1.txt
bash_homework/bash_homework/bam_wig.sh
bash_homework/bash_homework/b.filter_random.pl
bash_homework/bash_homework/c1.txt
bash_homework/bash_homework/chrom.size
bash_homework/bash_homework/c.txt
bash_homework/bash_homework/d1.txt
bash_homework/bash_homework/dir.txt
bash_homework/bash_homework/.DS_Store
bash_homework/bash_homework/e1.txt
bash_homework/bash_homework/f1.txt
bash_homework/bash_homework/human_geneExp.txt
bash_homework/bash_homework/if.sh
bash_homework/bash_homework/image
bash_homework/bash_homework/insitiue.txt
bash_homework/bash_homework/mouse_geneExp.txt
bash_homework/bash_homework/name.txt
bash_homework/bash_homework/number.sh
bash_homework/bash_homework/out.bw
bash_homework/bash_homework/random.sh
bash_homework/bash_homework/read.sh
bash_homework/bash_homework/test3.sh
bash_homework/bash_homework/test4.sh
bash_homework/bash_homework/test.sh
bash_homework/bash_homework/test.txt
bash_homework/bash_homework/wigToBigWig
bash_homework/__MACOSX/._bash_homework
bash_homework/__MACOSX/bash_homework/._a1.txt
bash_homework/__MACOSX/bash_homework/._a-docker
bash_homework/__MACOSX/bash_homework/._app
bash_homework/__MACOSX/bash_homework/._a.txt
bash_homework/__MACOSX/bash_homework/._b1.txt
bash_homework/__MACOSX/bash_homework/._backup
bash_homework/__MACOSX/bash_homework/._bam_wig.sh
bash_homework/__MACOSX/bash_homework/._b.filter_random.pl
bash_homework/__MACOSX/bash_homework/._bin
bash_homework/__MACOSX/bash_homework/._biosoft
bash_homework/__MACOSX/bash_homework/._c1-RBPanno
bash_homework/__MACOSX/bash_homework/._c1.txt
bash_homework/__MACOSX/bash_homework/._chrom.size
bash_homework/__MACOSX/bash_homework/._c.txt
bash_homework/__MACOSX/bash_homework/._d1.txt
bash_homework/__MACOSX/bash_homework/._datatable
bash_homework/__MACOSX/bash_homework/._db
bash_homework/__MACOSX/bash_homework/._dir.txt
bash_homework/__MACOSX/bash_homework/._download
bash_homework/__MACOSX/bash_homework/._.DS_Store
bash_homework/__MACOSX/bash_homework/._e1.txt
bash_homework/__MACOSX/bash_homework/._e-annotation
bash_homework/__MACOSX/bash_homework/._exRNA
bash_homework/__MACOSX/bash_homework/._f1.txt
bash_homework/__MACOSX/bash_homework/._genome
bash_homework/__MACOSX/bash_homework/._git
bash_homework/__MACOSX/bash_homework/._highcharts
bash_homework/__MACOSX/bash_homework/._home
bash_homework/__MACOSX/bash_homework/._hub29
bash_homework/__MACOSX/bash_homework/._human_geneExp.txt
bash_homework/__MACOSX/bash_homework/._ibme
bash_homework/__MACOSX/bash_homework/._if.sh
bash_homework/__MACOSX/bash_homework/._image
bash_homework/__MACOSX/bash_homework/._insitiue.txt
bash_homework/__MACOSX/bash_homework/._l-lwl
bash_homework/__MACOSX/bash_homework/._map2
bash_homework/__MACOSX/bash_homework/._mljs
bash_homework/__MACOSX/bash_homework/._module
bash_homework/__MACOSX/bash_homework/._mogproject
bash_homework/__MACOSX/bash_homework/._mouse_geneExp.txt
bash_homework/__MACOSX/bash_homework/._name.txt
bash_homework/__MACOSX/bash_homework/._node_modules
bash_homework/__MACOSX/bash_homework/._number.sh
bash_homework/__MACOSX/bash_homework/._out.bw
bash_homework/__MACOSX/bash_homework/._perl5
bash_homework/__MACOSX/bash_homework/._postar2
bash_homework/__MACOSX/bash_homework/._postar_app
bash_homework/__MACOSX/bash_homework/._postar.docker
bash_homework/__MACOSX/bash_homework/._random.sh
bash_homework/__MACOSX/bash_homework/._RBP_map
bash_homework/__MACOSX/bash_homework/._read.sh
bash_homework/__MACOSX/bash_homework/._rout
bash_homework/__MACOSX/bash_homework/._script
bash_homework/__MACOSX/bash_homework/._script_backup
bash_homework/__MACOSX/bash_homework/._software
bash_homework/__MACOSX/bash_homework/._tcga
bash_homework/__MACOSX/bash_homework/._test
bash_homework/__MACOSX/bash_homework/._test3.sh
bash_homework/__MACOSX/bash_homework/._test4.sh
bash_homework/__MACOSX/bash_homework/._test.sh
bash_homework/__MACOSX/bash_homework/._test.txt
bash_homework/__MACOSX/bash_homework/._tmp
bash_homework/__MACOSX/bash_homework/._tmp_script
bash_homework/__MACOSX/bash_homework/._var
bash_homework/__MACOSX/bash_homework/._wigToBigWig
bash_homework/__MACOSX/bash_homework/._x-rbp
```