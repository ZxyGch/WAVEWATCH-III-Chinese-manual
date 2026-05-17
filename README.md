# WAVEWATCH III manual v7.14

> 手册的 PDF 版在 https://github.com/ZxyGch/WAVEWATCH-III-Chinese-manual/releases/tag/v7.14 

这个目录是 WAVEWATCH III 的用户手册和系统文档源码目录。原始手册由一组
LaTeX 文件组成，入口文件是 `manual.tex`，默认生成英文版 `manual.pdf`。

本仓库还包含一份中文翻译版手册。中文入口文件是：

```sh
manual_zh.tex
```

中文翻译正文集中放在 `zh/` 目录下，并尽量镜像原手册目录结构，例如：

```sh
zh/intro_zh.tex
zh/eqs_zh.tex
zh/num_zh.tex
zh/run_zh.tex
zh/impl_zh.tex
zh/sys_zh.tex
zh/app/*.tex
```

原英文文件仍保留在 `intro/`、`eqs/`、`num/`、`run/`、`impl/`、`sys/`、
`app/` 等目录中，便于对照和继续维护。

## 依赖

在当前机器上，下面这些命令已经可用，可以直接执行本文档中的构建命令：

```sh
make
latex
xelatex
bibtex
dvipdf
```

如果换到一台新机器，需要先安装一个完整的 LaTeX 发行版。macOS 上通常可安装
MacTeX；Linux 上通常可安装 TeX Live，并确保包含 `xelatex`、`bibtex`、
`ctex` 和 Fandol 中文字体。英文版还会用到 `latex` 和 `dvipdf`。

macOS（Homebrew）：

```sh
brew install --cask mactex
echo 'export PATH="/Library/TeX/texbin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Ubuntu / Debian（完整安装，最省心但体积较大）：

```sh
sudo apt update
sudo apt install -y make texlive-full
```

Fedora / RHEL：

```sh
sudo dnf install -y make texlive-scheme-full
```

安装后可用下面的命令检查：

```sh
which make latex xelatex bibtex dvipdf
```



## 生成英文 PDF

英文版沿用原工程的 makefile：

```sh
make
```

默认会生成：

```sh
manual.pdf
```

如需先生成手册中引用的示例输入片段和脚本帮助输出，可运行：

```sh
make inp
```

原 makefile 需要 `latex`、`bibtex`、`dvipdf` 在 `PATH` 中可用，并需要
`INPDIR`/`NMLDIR` 指向 WAVEWATCH III 的示例输入文件目录。默认值通常为
`../model/inp` 和 `../model/nml`。

## 生成中文 PDF

```sh
xelatex -interaction=nonstopmode -halt-on-error manual_zh.tex
bibtex manual_zh
xelatex -interaction=nonstopmode -halt-on-error manual_zh.tex
xelatex -interaction=nonstopmode -halt-on-error manual_zh.tex
```

生成结果为：

```sh
manual_zh.pdf
```

如果只想快速预览，也可以先运行两遍 XeLaTeX：

```sh
xelatex -interaction=nonstopmode -halt-on-error manual_zh.tex
xelatex -interaction=nonstopmode -halt-on-error manual_zh.tex
```

不过参考文献和交叉引用可能不会完全更新。

## 清理生成文件

原 makefile 的清理命令为：

```sh
make clean
```

注意：`make clean` 会删除编译生成的 `.aux`、`.log`、`.out`、`.toc`、
`.bbl`、`.blg`、`.dvi`、`.pdf` 等文件，也会删除 `make inp` 生成的示例
片段。清理后如果要重新生成中文 PDF，需要再次运行
`make ww3_gspl.sh.out run_test.out`。
