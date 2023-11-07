---
title: 在hexo的matery主题下显示数学公式
top: false
cover: false
author: DULULU oO
date: 2021-09-29 18:58:49
password:
summary:
tags: 
    - hexo
    - Markdown
categories: 计算机网络
---

Markdown中激活mathjax插件，并使用数学公式，如矩阵与各种数学符号

## 激活mathjax插件

主题config.yml文件里找到mathjax,改为true
由于mathjax的渲染时间较长，故勾选全局mathjax为true后还需要在每篇文章的最上方输入mathjax:true


此时就可以输入数学公式了，以矩阵为例

输入
```Latex
$$\begin{bmatrix}             
        1&1&0\\                             
        1&0&1\\
        \end{bmatrix}$$

```

以$$\begin开始,以\end{}$$结束
但由于matery主题的限制，矩阵不会换行，因此考虑使用markdown it Plus代替Marked渲染器

## 利用Katex

```shell
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```
卸载marked，安装markdown-it-plus


```shell
npm un hexo-renderer-marked --save
npm i @upupming/hexo-renderer-markdown-it-plus --save
```
也可以考虑吧直接用升级版

再修改hexo的config.yml文件
```yml
markdown_it_plus:
  render:
    html: true
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
  anchors:
    level: 2
    collisionSuffix: 'v'
    permalink: true
    permalinkClass: header-anchor
    permalinkSide: 'left'
    permalinkSymbol: ¶
```
就完成了配置
可以用
```Latex
$$\begin{bmatrix}             
        1&1&0\\                             
        1&0&1\\
        \end{bmatrix}$$

```
再测试一下，此时的matrix会分行了

## Markdown数学公式的使用

### 基础格式
```Latex
$$\begin{equation}
公式\\
公式\\
\end{equation}$$
```

### 上下标

**^**表示上标，**_**表示下标，需要用**{}** 

```Latex
$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$
```

结果
![结果](/img/posts/Markdown/index.jpg)

### 矩阵表示

```Latex
$$\begin{matrix}
0&1&1\\
1&1&0\\
1&0&1\\
\end{matrix}$$
```
![矩阵表示](/img/posts/Markdown/matrix0.jpg)

在起始、结束标记用下列词替换 matrix
- pmatrix：小括号边框
- bmatrix：中括号边框
- Bmatrix：大括号边框
- vmatrix：单竖线边框
- Vmatrix：双竖线边框

![边框矩阵表示](/img/posts/Markdown/matrix1.jpg)

### 方程组表示

```Latex
$$\begin{cases}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2\\
a_3x+b_3y+c_3z=d_3\\
\end{cases}
$$
```
![方程组表示](/img/posts/Markdown/equation0.jpg)
