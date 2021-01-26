# 论文源码仓库示例

本仓库是论文源码仓库示例, 以作参考。论文的源码管理是必要的: 一来方便自己, 代码复用, 减少重复的工作量; 二来是方便他人, 复现实验结果, 提升论文的可信度。对于源码的管理, 不强求但推荐使用`Git`工具。其好处在于方便追踪代码的开发历史, 方便回溯或与他人协作。在此不详述`Git`的使用方法, 可以参考[Resources to learn Git](https://try.github.io/)。此外, 云端可使用`GitHub`或`GitLab`同步, 目前这两个平台都支持私有仓库的免费创建。


## 仓库结构

下面给出一个常见的MATLAB项目仓库结构, 各个文件夹的用途说明如下：

```shell
.
├── exp             // 用于存储实验脚本
├── figure          // 用于存储实验结果图
├── lib             // 用于存储常用的函数文件, function
├── result          // 用于存储实验结果
├── .gitignore      // git中略过的文件, 文件夹或某类文件后缀
├── README.md       // 本文档
└── ...             // 其他脚本
```

一般地, 遵循以下的几个指导原则：

+ 将算法封装为函数(`function`), 将可调整的参数作为函数的输入参数
+ 尽可能的模块化, 提高代码复用率
+ 将遍历参数的实验脚本存入`exp`文件夹
+ 将`function`存入`lib`
+ 程序执行末尾加上存储实验结果的脚本
+ 统一绘图脚本与实验脚本, 方便快速复现

有了相对固定的仓库结构, 方便理清头绪, 快速查找、复现实验结果。

## 文档撰写

### 语法

推荐使用`Markdown`语法, Markdown的优点是简单快捷, 并受GitHub/GitLab等云端`Git`平台原生支持渲染, 可以快速写出结构清晰的文档, 借助一些辅助工具<sup style="font-size: 0.6em">[1],[2]</sup>还可以增加对$\textrm{\LaTeX}$公式的支持。

### 环境声明

文档需要写明代码执行环境, 对于MATLAB而言, 一般可说明版本, 而对于需要用到其他的包时, 也需要相应声明包名称及版本, 例如常见的: cvx。写法如下:

```
+ Matlab: R2020a
+ CVX: version 2.2
```

此外, 如果用到了python, 最好将其中用到的包列出, 或着如果用的是conda管理python环境可借助如下命令导出所用python环境的列表:

```shell
conda env export > environment.yml
```

将导出的`environment.yml`附在repo中。

<hr style="width:20%;text-align:left;margin-left:0;margin-top:0px;height:0.5%">
<div style="margin: 0 auto; margin-top: -25px; font-size: 0.7em" markdown="1">

[1]. [leegao/readme2tex: Renders LaTeX for Github Readmes](https://github.com/zouyu4524/readme2tex)
[2]. [Markdown PDF](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf)
</div>


## 代码示例

编写的代码尽可能做到每个文件**自我说明**, 注明关键代码的说明。

```matlab
% We consider the most basic convex optimization problem, least-squares 
% (also known as linear regression). In a least-squares problem, we seek 
% x \in R^n that minimizes ||Ax−b||_2, where A \in R^{mxn} is skinny and 
% full rank (i.e., m >= n and Rank(A)=n). Let us create the data for a 
% small test problem in Matlab:

m = 16; n = 8;
A = randn(m,n);
b = randn(m,1);

% Then the least-squares solution x=(A^T*A)^{-1} * A^T * b is easily 
% computed using the backslash operator:

x_ls = A \ b;

% Using CVX, the same problem can be solved as follows:

cvx_begin
    variable x(n)
    minimize( norm(A*x-b) )
cvx_end
```