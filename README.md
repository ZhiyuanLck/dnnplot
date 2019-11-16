# `dnnplot`
Plot 3d blocks in deep neural networks. Less code, better performance and easier control.

## Load package

Add the code below to the preamble.

```tex
\usepackage{dnnplot}
```

**Note**: `dnnplot` does not load `tikz` automatically. You need to load it manually.

## Basic usage

With `dnnplot`,  you are suppose to draw blocks from left to right, down to up and back to front, which are posit directions of x, y,​ and ​z​ axes respectively.



The block in `dnnplot` is a customized shape, which means you can use it as a node with lots of anchors and all the styles that can be set for node. The grammar is simple:

```tex
\node[block={<specification>}];
```

where `specification` is a list of `keys/values`.

Here is a primary example (MWE):

```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \node[block={scale=1, x=30mm, y=30mm, z=30mm}, anchor=back south west] at (0, 0) {};
  \draw[->, thick] (0, 0) -- (4, 0) node[below] {$x$};
  \draw[->, thick] (0, 0) -- (0, 4) node[right] {$y$};
  \draw[->, thick] (0, 0) -- (-2, -2) node[left] {$z$};
\end{tikzpicture}
\end{document}end{document}
```

We plot a base 3d block. 

<p align="center">
<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_base.png" alt="Basic use"  width=250>
	<p align="center">
		<em>plot base block</em>
	</p>
</p>

## Available anchors

A variety of anchors have been defined. They are all shown as below.

```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \newcommand{\frontanchors}{front, front north east, front north west, front south east, front south west}
  \newcommand{\backanchors}{back, back north east, back north west, back south east, back south west}
  \newcommand{\centeranchors}{center, north east, north west, south east, south west}
  \tikzset{anchors/.style={fill=red, block={back plot, scale=1, x=5cm, y=5cm ,z=5cm}}}
  \node[anchors] (a) {};
  \foreach \a in \frontanchors{
    \fill[fill=red] (a.\a) circle (2pt) node[below] {\a};
  }
  \foreach \a in \centeranchors{
    \fill[fill=blue] (a.\a) circle (2pt) node[below] {\a};
  }
  \foreach \a in \backanchors{
    \fill[fill=green] (a.\a) circle (2pt) node[below] {\a};
  }
\end{tikzpicture}
\end{document}end{document}
```



<p align="center">
<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_anchor1.png" alt="Anchors"  width=500>
	<p align="center">
		<em>anchors</em>
	</p>
</p>

## Annotation

I define a command to annotate easily:

```tex
\blocklabel[<optional style>]{<node name>}{<edge specification>}{<text>}
```

- `<node name>`: specified when you declare a node such as `\node (a) {};` then the `node name` is `a`.

- `<edge specification>`: specify the edge to which the label are attached. Available `edge specifications` such as `upperright` are shown in the figure below. If you pass a undefined specification, nothing will happen.

- `<text>`: text to be shown in the node.

- `<optional style>`: all the arguments are passed to the text node. If it is empty, default position of the label are set.  Options like `near start` also work.

**Note: no semicolon is needed in this command!**

```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \tikzset{labels/.style={fill=red, block={back plot, scale=1, x=5cm, y=5cm ,z=5cm}}}
  \node[labels] (a) {};
  \foreach \l in {fronteast, frontwest, frontnorth, frontsouth, backeast,
    backwest, backnorth, backsouth, upperleft, upperright, lowerleft, lowerright}
  {\blocklabel[font=\itshape]{a}{\l}{\l~label}}
\end{tikzpicture}
\end{document}end{document}
```

  

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_annotation1.png" alt="Labels"  width=500>
	<p align="center">
		<em>annotation</em>
	</p>
</p>

## Shift in z axis

1. You can use `block={zshift=<distance>}` to apply a shift in z axis.

```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \tikzset{A/.style={block={back plot, scale=1, x=5cm, y=5cm ,z=5cm, #1}}}
  \draw[->, thick] (0, 0) -- (6, 0) node[below] {$x$};
  \draw[->, thick] (0, 0) -- (0, 6) node[right] {$y$};
  \draw[->, thick] (0, 0) -- (-3.5, -3.5) node[left] {$z$};
  \node[A, fill=red, anchor=back south west] (a) at (0, 0) {};
  \node[fill=green, A={zshift=2.5cm}] at (a) {}; % shift half of width in z axis
\end{tikzpicture}
\end{document}end{document}
```

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_zshift.png" alt="zshift"  width=400>
	<p align="center">
		<em>shift in z axis</em>
	</p>
</p>

2. Tikz library `positioning` only supports shifting in 2d space. With `dnnplot` you can use `block={pre=<node name>, front=<distance>}` to carry out the function just like `right=2cm of a` in `positioning`.
```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \tikzset{
    base/.style n args={4}{
      block={scale=1, x=#1mm, y=#2mm, z=#3mm, #4}
    },
    A/.style={fill=red, base={20}{20}{50}{#1}},
    B/.style={fill=blue, base={20}{20}{20}{#1}},
  }
  \node[A] (a) {};
  \node[B={pre=a, front=0mm}] (b) {};
  \node at (a.north) {A};
  \node at (b.north) {B};
\end{tikzpicture}
\end{document}end{document}
```

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_front.png" alt="front"  width=400>
	<p align="center">
		<em>front of a block</em>
	</p>
</p>

## Alignment

### Arbitrary alignment

You can apply arbitrary alignment with the option `block={pre=<node name>, align axis=<axis>, hfactor=<value>, vfactor=<value>}`. By default, center alignment is applied.

1. `block={align axis=<axis>}`, `align axis` is set to `x` by default.
- `align axis=x`: you are going to apply the alignment in the way that you are looking at the referenced block **from right to left**, and then you wish to align current block among left edge, right edge, ... etc. or just let the block to be center aligned.
	- `align axis=y`: you are going to apply the alignment in the way that you are looking at the referenced block **from top to bottom**.
	- `align axis=z`: you are going to apply the alignment in the way that you are looking at the referenced block **from front to  back**.
<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_align_axis.png" alt="align axis"  width=500>
	<p align="center">
		<em>how does alignment work</em>
	</p>
</p>
Figure above shows how the alignment works. The code is in `examples/dnnplot_align_axis.tex`.

2.  `hfactor=<value>` and `vfactor=<value>`
   - Understanding the effect of these two parameters, you are supposed to learn something about **how does the alignment work in `dnnplot`**:
     - First,  direction of the view is set by e. g. `align axis=x`, which means you look at y-z plane from right to left. Suppose the rectangle *D* is obtained by shifting the right side of the referenced block so that it passes through the center of current block. Initially, we fix the center current block at the lower left corner of *D*, where `hfactor=0, vfactor=0`.
     - Then, horizon shift and vertical shift are carried out. If the block are wished to be horizontally shifted to the lower right corner of *D*, corresponding `hfactor` should be set to `1`. Same as `vfactor` if you want to shift the block vertically to the upper left corner.  We can define `hshift` (not a option) to be the shift distance from the lower left corner to lower right corner and `vshift` is similarly defined. 
     - The eventual effect is that you settle the block in the lower left corner, and shift the block `hfactor*hshift` horizontally and `vfactor*vshift` vertically. It is just like normalized coordinates. Of cause, you can set the value of factor out of `[0, 1]`, then the block will be shifted out of the border of *D*.

Now we can plot some fancy blocks like this:

```tex
\documentclass[tikz, border=4mm]{standalone}
\usepackage{xcolor}
\usetikzlibrary{positioning}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}
  \tikzset{
    base/.style n args={4}{
      block={scale=1, x=#1mm, y=#2mm, z=#3mm, #4}
    },
    A/.style={fill=red, base={200}{10}{200}{#1}},
    B/.style 2 args={},
    B/.code 2 args={
      \pgfmathsetmacro\tempa{100*#1}
      \pgfmathsetmacro\tempb{100*#2}
      \pgfkeysalso{
        above=2cm of a, fill=red!\tempa!green!\tempb!blue,
        base={10}{10}{10}{align axis=y, hfactor=#1, vfactor=#2, pre=a},
        fill opacity=0.5,
      }
    }
  }
  \node[A] (a) {};
  \foreach \h in {0, 0.1, ..., 1.1}{
    \foreach \v in {0, 0.1, ..., 1.1}{
      \node[B={\h}{\v}] {};
    }
  }
\end{tikzpicture}
\end{document}
```

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_fancy_block.png" alt="fancy blocks"  width=500>
	<p align="center">
		<em>fancy blocks</em>
	</p>
</p>

### Quick alignment

You can quickly apply alignment of common styles quickly by the way like this:

```tex
block={pre=<node name>, align axis=<axis>, align=<align specification}
```

- `<node name>`: name of the defined node.

- `<axis>`: apply the alignment among `<axis>`.

- `<align specification>`: specify the style of the alignment. Now it supports `center, left, right, up, down, upperleft, upperright, lowerleft, lowerright`. Actually, if you pass other undefined `<node name>` or `<align specification>`, it is just like passing `center`.

  **Note**: `<align specification>` has **higher priority**! If you have set `<align specification>`, setting of `hfactor` and `vfactor` will be ignored.

```tex
\documentclass[tikz, border=4mm]{standalone}
\usetikzlibrary{positioning, calc}
\usepackage{dnnplot}
\begin{document}
\begin{tikzpicture}[font=\large\bfseries, line width=1pt]
  \tikzset{
    base/.style n args={4}{
      block={scale=0.5, x=#1mm, y=#2mm, z=#3mm, #4}
    },
    A/.style={fill=red, base={10}{80}{150}{#1}},
    B/.style={fill=teal,right=1cm of a, base={10}{20}{30}{#1}},
  }
  \node[A] (a) {};
  \foreach \a [count=\i] in {center, left, right, up, down, upperleft, upperright, lowerleft, lowerright}{
    \node[B={pre=a, align=\a}] (\i) {};
    \draw[->] let \p\i=(\i.east) in (\i.east) -- (4cm, \y\i) node[right] {\a};
  }
\end{tikzpicture}
\end{document}
```



<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_align.png" alt="Alignment"  width="400">
	<p align="center">
		<em>alignment</em>
	</p>
</p>

### Shift and align the blocks in different axes

What will happen if you set `align axis=y`, but shift the block among x axis? Here is a example.

```tex
\documentclass[tikz, border=4mm]{standalone}
\usetikzlibrary{positioning}
\usepackage{dnnplot}
\usepackage{amsmath, bm}
\begin{document}
\begin{tikzpicture}[font=\large\bfseries, line width=1pt]
  \tikzset{
    base/.style n args={4}{
      block={scale=0.5, x=#1mm, y=#2mm, z=#3mm, #4}
    },
    A/.style={fill=red, base={50}{80}{150}{#1}},
    B/.style={base={10}{20}{30}{#1}},
  }
  \node[A, anchor=front south west] (a) at (0, 0) {};
  \foreach \axis/\color/\d in {x/teal/3.5cm, y/cyan/8cm, z/orange/13cm}{
    \begin{scope}[local bounding box=box-\axis]
    \foreach \align [count=\i] in {center, left, right, up, down, upperleft, upperright, lowerleft, lowerright}{
      \node[B={pre=a, align axis=\axis, align=\align}, fill=\color, right=\d of a] {};
    }
    \end{scope}
    \node at (0, -1 -| box-\axis) {align axis=$\bm{\axis}$};
  }
\end{tikzpicture}
\end{document}
```

<p align="center">
	<img src="https://zhiyuan13-1258455953.cos.ap-chengdu.myqcloud.com/dnnplot/dnnplot_diff_axis.png" alt="Alignment"  width="800">
	<p align="center">
		<em>alignment among different axis</em>
	</p>
</p>

### Note

All the alignment options should set `block={pre=<node name>}` first. Otherwise, nothing will happen!

## Plane block

If you want a plane block in x-y plane, letting `z=0cm` (which is default so you can just ignore this option) seems to be a feasible way, but it may cause some little problem such as some lines are drawn a little wider. So, you'd better add the `block={plane}` option to avoid the case.

## Options

### block

- `x`: default 0cm.
- `y`: default 0cm.
- `z`: default 0cm.
- `scale`: apply a scaling to the block, default 0.5.
- `pre`: name of the previous node, used for shifting and alignment.
- `align axis`: axis to be aligned among.
- `hfactor`: coefficient of `hshift`.
- `vfactor`: coefficient of `vshift`.
- `align`: quick style of alignment.
- `back plot`: draw hidden edges  as dashed lines.
- `plane`: draw plane block, i. e.  3d rectangle.

### default node options

Some options of node are set by default. If you want to overload them, please add the option **after** `block={...}` in `node[block={...}]`.

- `inner sep=0`
- `draw`
- `fill opacity=.3`
- `thick`

## To be added...

1. grid block
2. more shape (or you can pull a request)

## Thanks

[Schrödinger's cat](https://tex.stackexchange.com/users/194703/schr%c3%b6dingers-cat) helps me a lot and provides some good ideas.

