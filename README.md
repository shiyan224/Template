# ACM Template
A generator for acm/icpc template manaual with Latex.

# 文件/目录结构

* 所有文件和目录的格式都按照`**_**`以及`**_**.cpp`的形式

* 所有文件都必须以UTF-8编码

* 文件目录最多为三级

```
.
├── 01_Section1
├── 02_Section2
|   ├── 01_Section2-Charpter1
|   └── 02_Section2-Charpter2
├── 03_Section3
|   └── 00_Section3-Charpter1(Hide)
├── 04_Section4
└── 05_Section5
```

* 生成tex文件时，按照命令行中'ls'的顺序生成各个章节

* 开头为`00_`时，不生成子标题，具体可见repo内的格式和pdf

* 代码开头两个```//  ---```中包含的内容，会去除注释符号后显示在代码前的正文部分，可以写数学公式等

# 编译pdf

* 使用xelatex编译

# 命令
```bash
$ python Manual.py
```
