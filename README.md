# dnnplot
Plot 3d blocks in deep neural networks. Less code, better performance and easier control.

## Note

`dnnplot` does not load `tikz` automatically. You need to load it manully.

## Basic use

Blocks’ placement is arranged as below. And you are suppose to draw blocks from left to right, down to up and back to front, which are posit directions of $x, y,$ and $z$ axes respectly.

<p align="center">
<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_main.png" alt="Basic use"  height="250">
	<p align="center">
		<em>parameter arrangement</em>
	</p>
</p>

The block in dnnplot is a cutomized shape, which means you can use it as a node with lots of anchors and all the styles that can be set for node. The grammar is simple:

```tex
\node[block={<specification>}];
```

where `specification` is a list of `keys/values`.

Here is a primary example (MWE):

```tex
\documentclass[tikz, border=1cm]{standalone}
\usepackage{dnnplot}
\begin{document}
  \tikz \node[block={scale=1, width=30mm, height=30mm, channel=30mm, back plot}] {};
\end{document}
```

We plot a base 3d block. And we use `back plot` option to allow hidden edges to be shown as dashed line.

<p align="center">
<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_base.png" alt="Basic use"  height="250">
	<p align="center">
		<em>plot base block</em>
	</p>
</p>

## Available anchors

A variety of anchors have been defined. They are all shown as below.

<p align="center">
<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_anchor.png" alt="Anchors"  height="400">
	<p align="center">
		<em>anchors</em>
	</p>
</p>

## Easy annotation

I define a command to annotate easily:

```tex
\blocklabel[<optional style>]{<node name>}{<edge specification>}{<text>}
```

- `<node name>`: specifed when you declare a node such as `\node (a) {};` then the `node name` is `a`.

- `<edge specification>`: specify the edge to which the labe are attached. Available `edge specifications` such as `upperright` are shown in the figure below. If you pass a undefined specification, nothing will happen.

- `<text>`: text to be shown in the node.

- `<optional style>`: all the arguments are passed to the text node. If it is empty, default position of the label are set.  Options like `near start` also work.
**Note: no semicolon is needed in this command!**

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_annotation.png" alt="Labels"  height="400">
	<p align="center">
		<em>annotation</em>
	</p>
</p>

## Alignment

If you draw a block `(a)`, and then a block `(b)` using `right=3cm of a` option in tikzlibrary `positioning`,  by default `(a)` and `(b)` are central aligned in $x$ axis of the $3d$ space.

Primarily, I have implement the basic alignment in $x$ axis by setting

```
block={pre=<node name>, align=<align specification}
```

in node option.

- `<node name>`: name of the defined node.
- `<align specification>`: specify the style of the alignment. Now it supports `xleft, xright, xup, xdown`, which means applying alignment along the corresponding edge of previous node.
  **Note: if a wrong node name or specification is passed, the block is drawn in situ.**

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_align.png" alt="Alignment"  height="400">
	<p align="center">
		<em>alignment</em>
	</p>
</p>

## Options of block shape

- `width`: width of the block, default 1cm.
- `height`: width of the block, default 1cm.
- `channel`: number of channels of the block, default 1cm.
- `scale`: apply a scaling to the shape, default 0.5.
- `pre`: name of the previous node, used for shifting and alignment.
- `align`: specify the style of the alignment.
- `back plot`: if this option are used, hidden edges are drawn as dashed lines.

## To be added...

1. Shifting in $z$ axis
2. Arbitrary alignment

