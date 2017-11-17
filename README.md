
<p align="center"><img height="80" src="./vim-web-logo.png" /></p>

<p align="center">Only tested on Mac OSx</p>

<img align="center" src="./vim-web.gif">

目录
===

<details>
<summary>点击展开目录菜单</summary>

<!-- TOC -->

- [安装Vim-Web](#安装vim-web)
- [插件管理](#插件管理)
- [理解Vim](#理解vim)
- [常用快捷键](#常用快捷键)
- [基本配置说明](#基本配置说明)
- [基础使用](#基础使用)
  - [快捷键通配符](#快捷键通配符)
  - [插入命令](#插入命令)
  - [删除命令](#删除命令)
  - [定位命令](#定位命令)
  - [复制剪切](#复制剪切)
  - [多光标编辑](#多光标编辑)
  - [简单排版](#简单排版)
  - [刷新重载打开的文件](#刷新重载打开的文件)
  - [保存退出](#保存退出)
  - [整页翻页](#整页翻页)
  - [开关注释](#开关注释)
  - [工程文件菜单](#工程文件菜单)
    - [切割窗口](#切割窗口)
  - [Tab操作](#tab操作)
    - [多tab窗口拆分](#多tab窗口拆分)
    - [tab切换](#tab切换)
    - [HTML操作](#html操作)
    - [代码片段补全](#代码片段补全)
- [搜索查找替换](#搜索查找替换)
  - [搜索](#搜索)
  - [替换](#替换)
- [文件恢复](#文件恢复)
- [多文档编辑](#多文档编辑)
- [环境恢复](#环境恢复)
- [插件列表](#插件列表)
- [错误处理](#错误处理)
- [参考资料](#参考资料)
- [其它人的vimrc配置](#其它人的vimrc配置)

<!-- /TOC -->

</details>

## 安装Vim-Web

最新版本的Vim 7.4+  使用(`brew install macvim`)安装，vim 版本更新 `brew install macvim --override-system-vim`

<details>
<summary>1. 下载vim-web</summary>

将插件以及配置下载到 `~/.vim/` 目录中，这个目录是存放所有插件和配置的地方。vimscript是vim自己的一套脚本语言，通过这种脚本语言可以实现与 vim 交互，达到功能扩展的目的。一组 vimscript 就是一个 vim 插件，vim 的很多功能都由各式插件实现。

```shell
$ git clone https://github.com/jaywcjlove/vim-web.git ~/.vim
$ ln -s ~/.vim/.vimrc ~/.vimrc
# 创建插件安装目录 plugged
$ mkdir ~/.vim/plugged
```

或者脚本下载安装

```bash
# 安装 vim-web
curl -sLf https://raw.githubusercontent.com/jaywcjlove/vim-web/master/install | bash -s -- install
# 卸载 vim-web
curl -sLf https://raw.githubusercontent.com/jaywcjlove/vim-web/master/install | bash -s -- uninstall
```

</details>

<details>
<summary>2. 查看配置位置</summary>

```shell
# 进入vim输入下面字符
:echo $MYVIMRC
```

</details>

<details>
<summary>3. 插件下载安装</summary>

下载安装 `~/.vimrc` 中配置的插件，这个过程需要很长时间。

```shell
# 上面执行完成之后
# 开始下载安装插件
$ vim # 在vim中运行 ":PlugInstall"

```

</details>

<details>
<summary>4. 安装插件依赖</summary>

部分插件需要安装一些软件，vim的部分插件才起作用。

```bash
# 上面插件安装完成之后执行下面内容
# command-t 文件搜索插件安装
$ cd ~/.vim/plugged/command-t 
$ rake make

# 搜索文本内容工具
# 需要安装 CtrlSF的依赖ripgrep
$ brew install ripgrep

# 代码提示插件也需要你运行安装哦，不然没有效果嘞
$ cd ~/.vim/plugged/YouCompleteMe
$ ./install.py
# or 新版脚本过时了，推荐上面脚本
$ ./install.sh 

# 需要安装ctags 不然配置没效果哦
# ctags for Mac
$ brew install ctags
# ctags for Centos7
$ yum install ctags
```

**注：** 默认已经安装了前端必备插件。`.vimrc` 是控制 vim 行为的配置文件，位于 ~/.vimrc，不论 vim 窗口外观、显示字体，还是操作方式、快捷键、插件属性均可通过编辑该配置文件将 vim 调教成最适合你的编辑器。

</details>

<details>
<summary>5. 界面字体设置</summary>

`vim-powerline`状态栏主题，界面箭头需要安装[Powerline字体](https://github.com/powerline/fonts) （在我Mac上安装的是Source Code Pro for Powerline字体），下载安装完成之后，还需要你在命令行工具中设置该字体。

在iTerm2中设置方法：`Command+,` 进入偏好设置（Preferences）=> Profiles => Default(自己的主题配置) => Non-ASCII Font => Change Font(选择字体)

</details>

<details>
<summary>6. 启动Vim</summary>
打开命令行，输入命令行启动 `vim`。

```bash
$ vim
```

</details>

## 插件管理

这里面刚开始使用的Vim插件管理工具[VundleVim/Vundle.vim](https://github.com/VundleVim/Vundle.vim.git)，后面为了大家安装方便，使用了 [junegunn/vim-plug](https://github.com/junegunn/vim-plug)，这个插件管理工具，俺十分不喜欢，多了个 `autoload` 目录，安装过程也奇丑无比，安装快速，所以就使用它吧，下面命令更新安装的 `plug.vim`，默认已经有了不需要这一步。

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

<details>
<summary>安装插件</summary>

将配置信息其加入 `~/.vim/.vimrc` 中的`call plug#begin()` 和 `call plug#end()` 之间，最后进入 vim 输入下面命令，摁 `enter` 进行安装。

```shell
:PlugInstall
```

</details>

<details>
<summary>更新插件</summary>

插件更新频率较高，差不多每隔一个月你应该看看哪些插件有推出新版本，批量更新，只需启动Vim执行下面更新命令即可更新插件。

```shell
:PlugUpdate
```

</details>

<details>
<summary>卸载插件</summary>

先在 .vimrc 中注释或者删除对应插件配置信息，然后在 vim 中执行下面命令，即可删除对应插件。

```shell
:PlugClean
```

</details>

## 理解Vim

这部分来源 [一起来说 Vim 语](http://www.jianshu.com/p/a361ce8c97bc)，理解此部分是需要你已经了解了 Vim 的几种常用的工作模式（正常模式、插入模式、命令模式等）

<details>
<summary>动词理解</summary>

动词代表了我们打算对文本进行什么样的操作。例如：

```bash
d # 表示删除delete
r # 表示替换replace
c # 表示修改change
y # 表示复制yank
v # 表示选取visual select
```

</details>

<details>
<summary>名词理解</summary>

名词代表了我们即将处理的文本。Vim 中有一个专门的术语叫做 [文本对象] text object，下面是一些文本对象的示例：

```bash
w # 表示一个单词word
s # 表示一个句子sentence
p # 表示一个段落paragraph
t # 表示一个 HTML 标签tag
引号或者各种括号所包含的文本称作一个文本块。
```

</details>

<details>
<summary>介词理解</summary>

介词界定了待编辑文本的范围或者位置。

```bash
i # 表示在...之内 inside
a # 表示环绕... around
t # 表示到...位置前 to
f # 表示到...位置上 forward
```

</details>

<details>
<summary>数词理解</summary>

数词指定了待编辑文本对象的数量，从这个角度而言，数词也可以看作是一种介词。引入数词之后，文本编辑命令的语法就升级成了下面这样：

```
动词 介词/数词 名词
```

下面是几个例子：

```bash
c3w  # 修改三个单词：change three words
d2w  # 删除两个单词：delete two words
```

另外，数词也可以修饰动词，表示将操作执行 n 次。于是，我们又有了下面的语法：

```
数词 动词 名词
```

请看示例：

```bash
2dw # 两次删除单词（等价于删除两个单词）: twice delete word
3x  # 三次删除字符（等价于删除三个字符）：three times delete character
```

</details>

<details>
<summary>组词为句理解</summary>

有了这些基本的语言元素，我们就可以着手构造一些简单的命令了。文本编辑命令的基本语法如下：

```
动词 介词 名词
```

下面是一些例子（如果熟悉了上面的概念，你将会看到这些例子非常容易理解），请亲自在 Vim 中试验一番。

```bash
dip # 删除一个段落: delete inside paragraph
vis # 选取一个句子: visual select inside sentence
ciw # 修改一个单词: change inside word
caw # 修改一个单词: change around word
dtx # 删除文本直到字符“x”（不包括字符“x”）: delete to x
dfx # 删除文本直到字符“x”（包括字符“x”）: delete forward x
```

</details>

## 常用快捷键

这里的快捷键大部分是配置好的可用的，还有一部分是Vim自带的快捷键。

<details>
<summary>大小写切换</summary>

```bash
U # 选中 - 变大写 
u # 选中 - 变小写
~ # 选中 - 变大写变小写，小写变大写
```

</details>

<details>
<summary>窗口菜单和Tab操作</summary>

```bash
nw  # 窗口切换
;lw # 跳转至右方的窗口
;hw # 跳转至左方的窗口
;kw # 跳转至上方的子窗口
;jw # 跳转至下方的子窗口

# 可以直接在Tab之间切换。
gt # 后一个Tab标签
gT # 前一个Tab标签
;q # 关闭一个标签

;fl # 【显示文件菜单】 file list
;bn # 正向遍历 buffer
;bp # 逆向遍历（光标必须在 buffer 列表子窗口外）
;bd # 关闭当前buffer（光标必须在 buffer 列表子窗口外）
;bb # 你之前所在的前一个 buffer）
```

</details>

<details>
<summary>快速文本内定位和移动</summary>

```bash
0   # 行首
$   # 行尾

ctrl + y # 向上一行
ctrl + e # 向下一行
ctrl + u # 向上半屏
ctrl + d # 向下半屏
ctrl + f # 下一页 f 就是`forword` 
ctrl + b # 上一页 b 就是`backward`  

ctrl + o # 上一个光标的位置
ctrl + i # 下一个光标的位置

# 快速文本内定位
;;b # 光标前代码定位
;;e # 光标后代码定位
;;f # 光标后代码定位 <搜索自负> 出现定位信息
;;F # 光标前代码定位 <搜索自负> 出现定位信息
```

</details>

<details>
<summary>书签设定, 标记并跳转</summary>

```bash
ma  # 设定/取消当前行名为 x 的标签
m,  # 自动设定下一个可用书签名
mda # 删除当前文件中所有独立书签
m?  # 罗列出当前文件中所有书签，选中后回车可直接跳转；
mn  #按行号前后顺序，跳转至下个独立书签；
mp  #按行号前后顺序，跳转至前个独立书签。
'a  # 跳到书签
'.  # 最后一次编辑的地方
```

</details>

<details>
<summary>列选中编辑</summary>

```bash
Ctrl+v   # 进入选中模式，`hjkl`方向键选择片区
Shift＋i # 进入列选择批量编辑
```

</details>

<details>
<summary>代码注释相关操作</summary>

```bash
;cc # 代码注释"//"
;cm # 代码段落注释"/**/"
;ci # 注释相反，注释的取消注释，没注释的注释
;cs # 段落注释，注释每行前面加"*"
;c$ # 光标开始到行结束的位置注释
;cA # 在行尾部添加注释符"//"
;cu # 取消代码注释
```

</details>

<details>
<summary>文本编辑搜索等操作</summary>

```bash
xp  # 左右交换光标处两字符的位置
:200,320 join # 合并第200~320行
J  # 选中多行合并

:r ~/git/R.js # 将文件内容导入到该文件中
:r !date    # 将当前编辑时间导入当前文本光标所在行
:!date      # 查看编辑时间

;sp # 选中搜索 - 文本中选中关键字
    # normal模式下 选中搜索 - 文本中选中关键字
;sl # 选中搜索 - 结果列表

;y  # 复制到剪切板
y   # 复制
yy  # 复制当前行
nyy # n表示大于1的数字，复制n行
yw  # 从光标处复制至一个单子/单词的末尾，包括空格
ye  # 从光标处复制至一个单子/单词的末尾，不包括空格
y$  # 从当前光标复制到行末
y0  # 从当前光标位置（不包括光标位置）复制之行首
y3l # 从光标位置（包括光标位置）向右复制3个字符
y5G # 将当前行（包括当前行）至第5行（不包括它）复制
y3B # 从当前光标位置（不包括光标位置）反向复制3个单词
.   # 粘贴
p   # 粘贴

# 多光标编辑
Shift+n # 选中下一个相同字符
Shift+k # 跳过当前选中的字符

:1,24s/header/www/g  # 第1到24行将header替换成www
```

</details>

<details>
<summary>代码折叠缩进等操作</summary>

```bash
za # 单个代码折叠
zM # 折叠左右代码
zR # 所有代码折叠取消
;i   # 开/关缩进可视化- 代码缩进关联线条
;ig  # 上一条效果一样

>   # 代码缩进 - 选中摁尖括号
<   # 代码缩进 - 选中摁尖括号
```

</details>

<details>
<summary>其它一些快捷键</summary>


```bash
;t # 通过搜索文件打开文件

;ilt # 设置显示／隐藏标签列表子窗口(函数列表)的快捷键。速记：identifier list by tag

:!which ls  # 找命令不推出vim运行命令

<c-z>  # 退出Vim
```

</details>

## 基本配置说明

<details>
<summary>取消备份</summary>

```vim
set nobackup
set noswapfile
```

</details>

<details>
<summary>文件编码</summary>

```vim
set encoding=utf-8
```

</details>

<details>
<summary>显示行号</summary>

```vim
set number  " 开启行号显示
            " 显示绝对行号      set number
            " 取消显示绝对行号   set nonumber
            " 显示相对行号       set relativenumber
            " 取消显示相对行号   set norelativenumber
```

</details>

<details>
<summary>显示状态栏</summary>

```vim
set laststatus=2   " 总是显示状态栏
```

</details>

<details>
<summary>高亮显示当前 - 行/列/搜索结果</summary>

```vim
set cursorline    " 高亮显示当前 - 行
set cursorcolumn  " 高亮显示当前 - 列
set hlsearch      " 高亮显示搜索结果
```

</details>

<details>
<summary>取消换行</summary>

```vim
set nowrap
```

</details>

<details>
<summary>设置鼠标操作</summary>

```vim
set mouse=a    " 设置鼠标滚动
"set mouse=v   " 若要使用鼠标复制内容到剪切板
```

</details>

<details>
<summary>设置缩进</summary>

```vim
set cindent
set tabstop=2
set shiftwidth=2
```

</details>

<details>
<summary>光标定位行显示设置</summary>

```vim
" set scrolloff=7   " 在上下移动光标时，光标的上方或下方至少会保留显示的行数
" :se so=7          " 上面配置的简写
:se so=999          " 总在中间
```

</details>

<details>
<summary>定义快捷键</summary>

```vim
" 定义快捷键的前缀，即 <Leader>
let mapleader=";"

nmap LB 0     " 定义快捷键到行首 / “)” 页尾
nmap LE $     " 定义快捷键到行尾

vnoremap <Leader>y "+y      " 设置快捷键将选中文本块复制至系统剪贴板
nnoremap <Leader>p "+p      " 设置快捷键将系统剪贴板内容粘贴至vim
```

</details>

## 基础使用

- `inoremap` (Insert Mode)就只在插入(insert)模式下生效
- `vnoremap` (Visual Mode)只在visual模式下生效
- `nnoremap` (Normal Mode)就在normal模式下(狂按esc后的模式)生效
- 快捷键`<c-y>,` 表示(<kbd>Ctrl</kbd><kbd>y</kbd><kbd>,</kbd>)
- 快捷键`<S-n>` 表示(<kbd>Shift</kbd><kbd>n</kbd>)

### 快捷键通配符

快捷键通配符 `<leader>` 相当于是一个通用的命令符，默认好像是`\`，你可以在`.vimrc`中将他改为任意一个按键，在我们这个配置我改为了冒号`;`

```
" 定义快捷键的前缀，即 <Leader>
let mapleader=";"
```

### 插入命令

```bash
a # → 在光标所在字符后插入  
A # → 在光标所在字符尾插入  
i # → 在光标所在字符前插入  
I # → 在光标所在行行首插入  
o # → 在光标下插入新行  
O # → 在光标上插入新行  
```

### 删除命令

```bash
x   # → 删除光标所在处字符  
nx  # → 删除光标所在处n个字符  
dd  # → 删除光标所在行，
ndd # → 删除n行  
dG  # → 删除光标所在行到文件末尾内容  
D   # → 删除光标所在处到行尾内容  
:n1,n2d # → 删除指定范围的行 如：1,2d  
```

### 定位命令

```bash
:set number   #→ 设置行号 简写set nu  
:set nonu   #→ 取消行号  
gg  #→ 到第一行  
G   #→ 到最后一行  
nG  #→ 到第n行  
:n  #→ 到第n行  
S   #→ 移至行尾  
0   #→ 移至行尾  
hjkl #→ 前下上后  

w   #→ 到下一个单词的开头  
b   #→ 与w相反  
e   #→ 到下一个单词的结尾。  
ge  #→ 与e相反  

0   #→ 到行头  
^   #→ 到本行的第一个非blank字符  
$   #→ 到行尾  
g_  #→ 到本行最后一个不是blank字符的位置。  
fa  #→ 到下一个为a的字符处，你也可以fs到下一个为s的字符。  
t,  #→ 到逗号前的第一个字符。逗号可以变成其它字符。  
3fa #→ 在当前行查找第三个出现的a。  
F 和 T → 和 f 和 t 一样，只不过是相反方向。  

zz # 将当前行置于屏幕中间（不是转载…）  
zt # 将当前行置于屏幕顶端（不是猪头~）  
zb # 底端啦~  
```

### 复制剪切

> `yy` 和 p 的组合键，或者 `dd` 和 p 的组合键

```bash
yy    # → 复制当前行  
;y    # → 复制到剪切板
y     # → 选中复制
nyy   # → n表示大于1的数字，复制n行
dd    # → 剪切当前行  
ndd   # → 剪切当前行以下n 行  
yw    # → 从光标处复制至一个单子/单词的末尾，包括空格
ye    # → 从光标处复制至一个单子/单词的末尾，不包括空格
y$    # → 从当前光标复制到行末
y0    # → 从当前光标位置（不包括光标位置）复制之行首
y3l   # → 从光标位置（包括光标位置）向右复制3个字符
y5G   # → 将当前行（包括当前行）至第5行（不包括它）复制
y3B   # → 从当前光标位置（不包括光标位置）反向复制3个单词
p、P  # → 粘贴在当前光标所在行或行上  
2dd   # → 删除2行  
3p    # → 粘贴文本3次  
.     # → 粘贴
```

### 多光标编辑

借助 [vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors) 实现多光标编辑功能。首先选中一个单词，然后使用快捷键`Shift+n`，就会选中下一个一模一样的字符，`Shift+k`跳过选中，然后你可以进行编辑了。默认这个插件快捷键是`Ctrl+n`，可能会冲突，单在我这里没有冲突，操作`Shift+n`快捷键更舒服，你可以配置自己的快捷键

```
let g:multi_cursor_next_key='<S-n>'
let g:multi_cursor_skip_key='<S-k>'
```

### 简单排版

```bash
:ce(nter)  # 居中显示光标所在行
:ri(ght)   # 靠右显示光标所在行
:le(ft)    # 靠左显示光标所在行
J          # 将光标所在下一行合并到光标所在行

>>         # 光标所在行增加缩进(一个tab)
<<         # 光标所在行减少缩进(一个tab)

n>>        # 光标所在行开始的n行增加缩进
n<<        # 光标所在行开始的n行减少缩进
```

### 刷新重载打开的文件

```bash
:e  # 刷新当前文件
:e! # 强制刷新当前文件
```

### 保存退出

```
:w new_filename     # → 保存为指定文件  
:w   # → 保存修改  
:wq  # → 保存修改并推出  
ZZ   # → 快捷键，保存修改并推出  
:q!  # → 不保存修改推出  
:wq! # → 保存修改并推出（文件所有者，root权限的用户）  
```

### 整页翻页

```bash
ctrl-f # 下一页 f 就是`forword` 
ctrl-b # 上一页 b 就是`backward`  
```

### 开关注释

- `;cc`，注释当前选中文本，如果选中的是整行则在每行首添加 `//`，如果选中一行的部分内容则在选中部分前后添加分别 `/**/`；
- `;cu`，取消选中文本块的注释。

### 工程文件菜单

[scrooloose/nerdtree](https://github.com/scrooloose/nerdtree)

自定义快捷键

```shell
;fl          # 显示文件菜单 file list
```

自带快捷键

```bash
shift+i      # 显示/隐藏隐藏文件 
t       # 在新 Tab 中打开选中文件/书签，并跳到新 Tab
T       # 在新 Tab 中打开选中文件/书签，但不跳到新 Tab
i       # split 一个新窗口打开选中文件，并跳到该窗口
gi      # split 一个新窗口打开选中文件，但不跳到该窗口
s       # vsplit 一个新窗口打开选中文件，并跳到该窗口
gs      # vsplit 一个新 窗口打开选中文件，但不跳到该窗口

ctrl + w + h    # 光标 focus 左侧树形目录
ctrl + w + l    # 光标 focus 右侧文件显示窗口
ctrl + w + w    # 光标自动在左右侧窗口切换
ctrl + w + r    # 移动当前窗口的布局位置
o       # 在已有窗口中打开文件、目录或书签，并跳到该窗口
go      # 在已有窗口 中打开文件、目录或书签，但不跳到该窗口

!       # 执行当前文件
O       # 递归打开选中 结点下的所有目录
x       # 合拢选中结点的父目录
X       # 递归 合拢选中结点下的所有目录，收起当前目录树
e       # Edit the current dif

双击    相当于 NERDTree-o
中键    对文件相当于 NERDTree-i，对目录相当于 NERDTree-e

D       # 删除当前书签
#
P       # 跳到根结点
p       # 跳到父结点
K       # 跳到当前目录下同级的第一个结点
J       # 跳到当前目录下同级的最后一个结点
k       # 跳到当前目录下同级的前一个结点
j       # 跳到当前目录下同级的后一个结点

C       # 将选中目录或选中文件的父目录设为根结点
u       # 将当前根结点的父目录设为根目录，并变成合拢原根结点
U       # 将当前根结点的父目录设为根目录，但保持展开原根结点
r       # 递归刷新选中目录，刷新当前目录
R       # 递归刷新根结点，刷新根目录树
m       # 显示文件系统菜单
cd      # 将 CWD 设为选中目录

I       # 切换是否显示隐藏文件
f       # 切换是否使用文件过滤器
F       # 切换是否显示文件
B       # 切换是否显示书签
#
q       # 关闭 NerdTree 窗口
?       # 切换是否显示 Quick Help
```

#### 切割窗口

```bash
:new      # 水平切割窗口
:split    # 水平切割窗口(或者直接输入   :sp  也可以)
:vsplit   # 垂直切割( 也可以  :vs  )
```

### Tab操作

#### 多tab窗口拆分

```bash
:tabnew [++opt选项] ［＋cmd］ 文件            #建立对指定文件新的tab
:tabc      #关闭当前的tab
:tabo      #关闭所有其他的tab
:tabs      #查看所有打开的tab
:tabp      #前一个
:tabn      #后一个
```

#### tab切换

```bash
# 下面为自定义快捷键
tnew #新建tab
tn #后一个 tab
tp #前一个 tab

# 窗口切换
nw

# 标准模式下：
gt , gT #可以直接在tab之间切换。

# 还有很多他命令， 看官大人自己， :help table 吧。
Ctrl+ww # 移动到下一个窗口
# 或者 先按组合键ctrl+w ，然后都松开，然后通过j/k/h/l(等于vim移动的方向键) 来移动大哦哦左/上/下/右的窗口
Ctrl+wj #移动到下方的窗口
Ctrl+wk #移动到上方的窗口
```

#### HTML操作

便捷操作得益于插件[Emmet.vim](https://github.com/mattn/emmet-vim)。键入 `div>p#foo$*3>a` 然后按快捷键 `<c-y>,` – 表示 `<Ctrl-y>` 后再按逗号【<kbd>Ctrl</kbd><kbd>y</kbd><kbd>,</kbd>】。

按大写的 V 进入 Vim 可视模式，行选取上面三行内容，然后按键 <c-y>,，这时 Vim 的命令行会提示 Tags:，键入ul>li*，然后按 Enter。

```shell
<ctrl+y>d # 根据光标位置选中整个标签  
<ctrl+y>D # 根据光标位置选中整个标签内容  
<ctrl-y>n # 跳转到下一个编辑点  
<ctrl-y>N # 跳转到上一个编辑点  
<ctrl-y>i # 更新图片大小  
<ctrl-y>m # 合并成一行  
<ctrl-y>k # 移除标签对  
<ctrl-y>j # 分割/合并标签  
<ctrl-y>/ # 切换注释  
<ctrl-y>a # 从 URL 地址生成锚  
<ctrl-y>A # 从 URL 地址生成引用文本  
```

#### 代码片段补全

让vim 自动完成相同的代码片断，比如 if-else、switch。[UltiSnips](https://github.com/SirVer/ultisnips) 这个插件可以帮助我们完成这项艰巨的工作。UltiSnips 有一套自己的代码模板语法规则，如下：

```
snippet if "if statement" i
if (${1:/* condition */}) { 
    ${2:TODO} 
} 
endsnippet
```

新版 UltiSnips 并未自带预定义的代码模板，你可以从 [honza/vim-snippets](https://github.com/honza/vim-snippets) 获取各类语言丰富的代码模板，这种模版我将它存放到 `~/.vim/mysnippets/` 目录里面，然后在配置中指定名字，同时修改出发快捷键，因为默认的快捷键与YCM插件冲突，需要在配置中更改。如下：

```vim
let g:UltiSnipsSnippetDirectories=["mysnippets"] " 配置目录
let g:UltiSnipsExpandTrigger="<leader><tab>"     " 配置快捷键
let g:UltiSnipsJumpForwardTrigger="<leader><tab>"    " 配向前跳转快捷键
let g:UltiSnipsJumpBackwardTrigger="<leader><s-tab>" " 配向后跳转快捷键
```

## 搜索查找替换

### 搜索

<details>
<summary>文件搜索</summary>

搜索有两个插件可以使用 [wincent/command-t](https://github.com/wincent/command-t) 和 [junegunn/fzf](https://github.com/junegunn/fzf)，`fzf`没有下载下来，这里在使用 `command-t` ，使用的时候记得，进入目录 `cd ~/.vim/plugged/command-t` 运行 `rake make`。

```shell
;t # 启动搜索文件
```

</details>

<details>
<summary>搜索文本内容</summary>

[dyng/ctrlsf.vim](https://github.com/dyng/ctrlsf.vim)，在插件完成安装之后，需要安装另外的工具，才能运行

```shell
brew install ripgrep

# 上面ripgrep安装好了之后，在.vimrc中配置下面内容
# 快捷键速记法：search in project
let g:ctrlsf_ackprg = 'rg' 
# 设置快捷键
nnoremap <Leader>sp :CtrlSF<CR>
# 选中搜索 - 文本中选中关键字
vmap     <Leader>sp <Plug>CtrlSFVwordPath
# 选中搜索 - 结果列表
vmap     <Leader>sl <Plug>CtrlSFQuickfixVwordPath
```

基本使用方法

```shell
;sp  # 搜索快捷键
:CtrlSF pattern dir  # 如果后面不带 dir 则默认是 . 当前目录搜索 
# 使用 j k h l 浏览CtrlSP窗口  使用 Ctrl + j/k 在匹配项中跳转。
# 使用 q 则退出 CtrlSP窗口
# 使用 p 
```

基本搜索，这种搜索不需要依赖任何插件，输入 <kbd>/</kbd> 再输入需要搜索的内容，摁 <kbd>Enter</kbd> 键，将会高亮所有搜索的内容，在英文状态下摁 <kbd>n</kbd> 字母键向下查找，下次打开文件时，这些字符串仍然高亮显示，使用命令`:nohl`取消高亮显示。

`/pattern<Enter>`：向下查找pattern匹配字符串   
`?pattern<Enter>`：向上查找pattern匹配字符串，使用了查找命令之后，使用如下两个键快速查找：  
`n`：按照同一方向继续查找   
`N`：按照反方向查找   

```shell
/^abc<Enter>       # 查找以abc开始的行 
/test$<Enter>      # 查找以abc结束的行 
//^test<Enter>     # 查找^tabc字符串
:s/vivian/sky/     # 替换当前行第一个 vivian 为 sky
:s/vivian/sky/g    # 替换当前行所有 vivian 为 sky
:n,$s/vivian/sky/  # 替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky
:n,$s/vivian/sky/g # 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky
                   #（n 为数字，若 n 为 .，表示从当前行开始到最后一行）
:%s/vivian/sky/  #（等同于 :g/vivian/s//sky/） 替换每一行的第一个 vivian 为 sky
:%s/vivian/sky/g #（等同于 :g/vivian/s//sky/g） 替换每一行中所有 vivian 为 sky

:s#vivian/#sky/#      # 替换当前行第一个 vivian/ 为 sky/
:%s+/oradata/apras/+/user01/apras1+ 
#（使用+ 来 替换 / ）： /oradata/apras/替换成/user01/apras1/

:s/str1/str2/          # 用字符串 str2 替换行中首次出现的字符串 str1
:s/str1/str2/g         # 用字符串 str2 替换行中所有出现的字符串 str1
:.,$ s/str1/str2/g     # 用字符串 str2 替换正文当前行到末尾所有出现的字符串 str1
:1,$ s/str1/str2/g     # 用字符串 str2 替换正文中所有出现的字符串 str1
:g/str1/s//str2/g      # 功能同上

//<abc  # 查找以test开始的字符串 
/abc/>  # 查找以test结束的字符串 

$       # 匹配一行的结束
^       # 匹配一行的开始
/<      # 匹配一个单词的开始，例如//<abc<Enter>:查找以abc开始的字符串
/>      # 匹配一个单词的结束，例如/abc/><Enter>:查找以abc结束的字符串 

*       # 匹配0或多次
/+      # 匹配1或多次
/=      # 匹配0或1次

.       # 匹配除换行符以外任意字符    
/a      # 匹配一个字符
/d      # 匹配任一数字      
/u      # 匹配任一大写字母

[]      # 匹配范围，如t[abcd]s 匹配tas tbs tcs tds
/{}     # 重复次数，如a/{3,5} 匹配3~5个a
/( /)   # 定义重复组，如a/(xy/)b 匹配ab axyb axyxyb axyxyxyb ...
/|      # 或，如：for/|bar 表示匹配for或者bar

/%20c   # 匹配第20列
/%20l   # 匹配第20行

# 切换 向上和向下搜索
# 输入 / 摁 Enter键，再摁 n 字母键向，下查找
# 输入 ? 摁 Enter键，再摁 n 字母键向，上查找
```

上面是全文搜索，下面是简单的单行搜索

```shell
fx  # 到第一个x
2fx # 到第二个x
Fx  # 往回查找
```

vim搜索时默认是大小写敏感的，要想实现大小写不敏感的搜索，如果仅仅是对当前打开的文件设置就用`:set ignorecase`，而永久性的设置可以到vimrc配置文件中添加一行

```vim
set ignorecase
```

</details>

<details>
<summary>文本中快速移动</summary>

[Lokaltog/vim-easymotion](https://github.com/Lokaltog/vim-easymotion) 把满足条件的位置用 [;A~Za~z] 间的标签字符标出来，找到你想去的位置再键入对应标签字符即可快速到达。

```shell
;;b # 光标前代码定位
;;e # 光标后代码定位
;;f # 光标后代码定位 <搜索自负> 出现定位信息
;;F # 光标前代码定位 <搜索自负> 出现定位信息
```

</details>

### 替换 

<details>
<summary>替换取消</summary>

```bash
r # → 取代关闭所在处字符  
R # → 从光标所在处开始替换字符，摁ESC结束  
u # → 取消上一步操作  
ctrl + r # → 返回上一步  
```

</details>

<details>
<summary>快捷替换</summary>

可视化模式下选中其中一个，接着键入 ctrl-n，你会发现第二个该字符串也被选中了，持续键入 ctrl-n，你可以选中所有相同的字符串，把这个功能与 ctrlsf 结合。这个功能是上面已经提过的 [多光标编辑](#多光标编辑) 的一个插件提供的功能。默认的快捷键已经被替换掉了，`ctrl-n` 替换成了 `shift-n`，跳过选中`ctrl-k` 换成了`shift-n`。

```vim
let g:multi_cursor_next_key='<S-n>' " 选中下一个相同内容
let g:multi_cursor_skip_key='<S-k>' " 跳过当前这个选中
```

</details>

<details>
<summary>精确替换</summary>

vim 有强大的内容替换命令，进行内容替换操作时，注意：如何指定替换文件范围、是否整词匹配、是否逐一确认后再替换。

```
:[range]s/{pattern}/{string}/[flags]
```

- 如果在当前文件内替换，[range] 不用指定，默认就在当前文件内；
- 如果在当前选中区域，[range] 也不用指定，在你键入替换命令时，vim 自动将生成如下命令：`:'<,'>s/{pattern}/{string}/[flags]`
- 你也可以指定行范围，如，第三行到第五行：`:3,5s/{pattern}/{string}/[flags]`
- 如果对打开文件进行替换，你需要先通过 `:bufdo` 命令显式告知 vim 范围，再执行替换；
- 如果对工程内所有文件进行替换，先 `:args **/.cpp */*.h` 告知 vim 范围，再执行替换；
- 替换当前行第一个 `vivian/` 为 `sky/`，`#` 作为分隔符 `:s #vivian/#sky/# `
- `:%s/vivian/sky/g`（等同于 `:g/vivian/s//sky/g`） 替换每一行中所有 vivian 为 sky
- `:n,$s/vivian/sky/g` 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky

`:21,27s/^/#/g` 行首替换`#`替换（增加）掉  
`:ab mymail asdf@qq.com` 输入`mymail` 摁下空格自动替换成`asdf@qq.com`  

</details>

## 文件恢复

非正常关闭vi编辑器时会生成一个`.swp`文件，这个文件是为了避免同一个文件产生两个不同的版本。同时可以用作意外退出恢复历史记录。

```
vi -r {your file name}
rm .{your file name}.swp
```

## 多文档编辑

im 的多文档编辑涉及三个概念：buffer、window、tab，可以对应理解成视角、布局、工作区。vim 中每打开一个文件，vim 就对应创建一个 buffer，多个文件就有多个 buffer，但默认你只看得到最后 buffer 对应的 window，通过插件 [MiniBufExplorer](https://github.com/fholgado/minibufexpl.vim)可以把所有 buffer 罗列出来，并且可以显示多个 buffer 对应的 window。

```bash
* # 的 buffer 是可见的；
! # 表示当前正在编辑的 window；
```

如果你想把多个 window 平铺成多个子窗口可以使用 MiniBufExplorer 的 s 和 v 命令：在某个 buffer 上键入 s 将该 buffer 对应 window 与先前 window 上下排列，键入 v 则左右排列（光标必须在 buffer 列表子窗口内）。

```bash
d  # 在某个 buffer 上键入 d 删除光标所在的 buffer
v  # 则左右排列（光标必须在 buffer 列表子窗口内）
s  # 在某个 buffer 上键入 s 将该 buffer 对应 window 与先前 window 上下排列
```

打开了多个文档，会在窗口的上方生成一个文字版本的Tab，我们需要快速切换不同的文件，需要配置快捷键，将如下信息加入 .vimrc 中：

```vim
" 显示/隐藏 MiniBufExplorer 窗口
map <Leader>bl :MBEToggle<cr>
" buffer 切换快捷键
map <Leader>bn :MBEbn<cr>  " 正向遍历 buffer
map <Leader>bp :MBEbp<cr>  " 逆向遍历（光标必须在 buffer 列表子窗口外）
map <Leader>bd :MBEbd<cr>  " 关闭当前buffer（光标必须在 buffer 列表子窗口外）
map <Leader>bb :b#<cr>     " 你之前所在的前一个 buffer）
" 在某个 buffer 上键入 d 删除光标所在的 buffer（光标必须在 buffer 列表子窗口内）：
```

## 环境恢复

编辑环境保存与恢复一直是我使用Sublime的理由之一，vim 文档说 viminfo 特性可以恢复书签、session 特性可以恢复书签外的其他项，所以，请确保你的 vim 支持这两个特性，通过下面命令查看是否支持这两个特性：

```
vim --version | grep mksession
vim --version | grep viminfo
```

默认保存/恢复环境步骤如下

```bash
:wa                      # 第一步，保存所有文档
:mksession! my.vim       # 第二步，借助 session 保存当前环境
:wviminfo! my.viminfo    # 第三步，借助 viminfo 保存当前环境
:qa                      # 第四步，退出 vim
:source my.vim           # 第五步，恢复环境，进入 vim 后执行
:rviminfo my.viminfo
```

具体能保存哪些项，可由 sessionoptions 指定，另外，前面几步可以设定快捷键，在 .vimrc 中增加：

```vim
" 设置环境保存项
set sessionoptions="blank,buffers,globals,localoptions,tabpages,sesdir,folds,help,options,resize,winpos,winsize"
set undodir=~/.undo_history/  " 保存 undo 历史
set undofile                  " 缺省关闭，局部于缓冲区
map <leader>ss :mksession! my.vim<cr> :wviminfo! my.viminfo<cr>   " 保存快捷键
map <leader>rs :source my.vim<cr> :rviminfo my.viminfo<cr>        " 恢复快捷键
```

⚠️ sessionoptions 无法包含 undo 历史，你得先得手工创建存放 undo 历史的目录（如，.undo_history/）再通过开启 undofile 进行单独设置，一旦开启，每次写文件时自动保存 undo 历史，下次加载在文件时自动恢复所有 undo 历史，不再由 :mksession/:wviminfo 和 :source/:rviminfo 控制。

## 插件列表

这里面所有的插件，并不是都放到了我的 [.vimrc](./.vimrc) 文件中 .vimrc 配置文件中，是我个人喜欢并且习惯的配置。

<details>
<summary>插件管理工具</summary>

- [junegunn/vim-plug](https://github.com/junegunn/vim-plug)
- [VundleVim/Vundle.vim](https://github.com/VundleVim/Vundle.vim)

</details>

<details>
<summary>主题风格</summary>

- [vim-colors-solarized](https://github.com/altercation/vim-colors-solarized) 主题风格素雅 solarized
- [molokai](https://github.com/tomasr/molokai) 主题风格多彩 molokai
- [phd](https://github.com/vim-scripts/phd) 主题风格复古 phd

</details>

<details>
<summary>使用界面</summary>

- [Mango](https://github.com/goatslacker/mango.vim) A nice color scheme
- [VimAirline](https://github.com/bling/vim-airline) 美化状态栏偏好设置
- [vim-powerline](https://github.com/Lokaltog/vim-powerline) 美化状态栏
- [vim-airline](https://github.com/vim-airline/vim-airline) 美化状态栏和配置
- [vim-airline-themes](https://github.com/vim-airline/vim-airline-themes) airline主题

</details>

<details>
<summary>管理项目</summary>

- [NERDTree](https://github.com/scrooloose/nerdtree) Manage your project files
- [VimFugitive](https://github.com/tpope/vim-fugitive) Git 集成
- [VimGitGutter](https://github.com/airblade/vim-gitgutter) Git 集成，强烈推荐！
- [EditorconfigVim](https://github.com/editorconfig/editorconfig-vim) Shared coding conventions
- [command-t](https://github.com/wincent/command-t) 文件搜索
- [vim-signature](https://github.com/kshenoy/vim-signature) 书签可视化的插件
- [BOOKMARKS--Mark-and-Highlight-Full-Lines](https://github.com/vim-scripts/BOOKMARKS--Mark-and-Highlight-Full-Lines) 它可以让书签行高亮
- [tagbar](https://github.com/majutsushi/tagbar) 方法地图导航
- [indexer.tar.gz](https://github.com/vim-scripts/indexer.tar.gz) 自动生成标签并引入
  - [DfrankUtil](https://github.com/vim-scripts/DfrankUtil) 上面插件，依赖这个插件
  - [vimprj](https://github.com/vim-scripts/vimprj) 上面插件，依赖这个插件
- [ctrlsf.vim](https://github.com/dyng/ctrlsf.vim) 上下文插件，例如搜素到关键字，中间缩略，展示一段上下文
- [vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors) 多光标编辑功能
- [gen_tags.vim](https://github.com/jsfaint/gen_tags.vim) 生成，加载，更新ctags/gtags文件。
- [ybian/smartim](https://github.com/ybian/smartim) 解决中文输入法下面无法使用命令
- [Yggdroot/LeaderF](https://github.com/Yggdroot/LeaderF) 模糊查找神器，性能超越ctrlp

</details>

<details>
<summary>代码书写</summary>

- [NERDCommenter](https://github.com/scrooloose/nerdcommenter) 注释更容易
- [DrawIt](https://github.com/vim-scripts/DrawIt) ASCII art 风格的注释
- [VimTrailingWhitespace](https://github.com/bronson/vim-trailing-whitespace) 突出尾随空格
- [Syntastic](https://github.com/scrooloose/syntastic) 语法检查
- [VimEasyAlign](https://github.com/junegunn/vim-easy-align) 调整部分代码
- [VimMultipleCursors](https://github.com/terryma/vim-multiple-cursors) Write on multiple lines easily
- [VimJsBeautify](https://github.com/maksimr/vim-jsbeautify) Reformat JavaScript, HTML and JSON files
- [VimYankStack](https://github.com/maxbrunsfeld/vim-yankstack) Iterate over yanked stack on paste
- [VimSurround](https://github.com/tpope/vim-surround) Quoting and parenthesizing
- [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) 键而全的、支持模糊搜索的、高速补全的插件
- [VimForTern](https://github.com/marijnh/tern_for_vim) Smart JavaScript autocompletion
- [VimNode](https://github.com/moll/vim-node) Navigate through node.js code/modules
- [VimLint](https://github.com/syngan/vim-vimlint) Linter used by syntastic for VimL
- [VimLParser](https://github.com/ynkdir/vim-vimlparser) VimL parser (required by VimLint)
- [emmet-vim](https://github.com/mattn/emmet-vim) 提高HTML和CSS的工作流
- [vim-cpp-enhanced-highlight](https://github.com/octol/vim-cpp-enhanced-highlight) C++ 语法高亮支持
- [vim-indent-guides](https://github.com/nathanaelkane/vim-indent-guides) 相同缩进的代码关联起来
- [vim-fswitch](https://github.com/derekwyatt/vim-fswitch) 接口文件（MyClass.h）与实现文件（MyClass.cpp）快捷切换的插件
- [MiniBufExplorer](https://github.com/fholgado/minibufexpl.vim) 显示多个 buffer 对应的 window
- [wildfire.vim](https://github.com/gcmt/wildfire.vim) 快捷键选中 `<>`、`[]`、`{}` 中间的内容
- [gundo.vim](https://github.com/sjl/gundo.vim) 让你有机会撤销最近一步或多步操作
- [vim-easymotion](https://github.com/Lokaltog/vim-easymotion) 快速移动，两次 `<leader>` 作为前缀键
- [Shougo/neocomplete.vim](https://github.com/Shougo/neocomplete.vim) 强大的自动补全插件
- [vim-instant-markdown](https://github.com/suan/vim-instant-markdown) 编辑 markdown 文档，自动开启 firefox 为你显示 markdown 最终效果
- [fcitx.vim](https://github.com/lilydjwg/fcitx.vim) 中/英输入平滑切换
- [othree/xml.vim](https://github.com/othree/xml.vim) 中/提供快速编写xml/html的能力，如标签自动闭合等
- [pangloss/vim-javascript](https://github.com/pangloss/vim-javascript) 提供js代码的智能缩进，仅使用了他的indent功能

</details>


<details>
<summary>代码阅读</summary>

- 语法高亮
  - [vim-polyglot](https://github.com/sheerun/vim-polyglot) 支持常见的语法高亮
  - [VimJson](https://github.com/elzr/vim-json) JSON 高亮和隐藏引号
  - [vim-jsx](https://github.com/mxw/vim-jsx) JSX语法高亮
  - [YaJS](https://github.com/othree/yajs.vim) JavaScript 语法 (ES5 and ES6)
  - [vim-css3-syntax](https://github.com/hail2u/vim-css3-syntax) CSS3 高亮，包括stylus,Less,Sass
  - [vim-css-color](https://github.com/skammer/vim-css-color) css高亮颜色
  - [gko/vim-coloresque](https://github.com/gko/vim-coloresque) css高亮颜色
  - [ScssSyntax](https://github.com/cakebaker/scss-syntax.vim) SCSS syntax
  - [HTML5](https://github.com/othree/html5.vim) HTML5 syntax
  - [Stylus](https://github.com/wavded/vim-stylus) Stylus 代码高亮
- [JavaScriptLibrariesSyntax](https://github.com/othree/javascript-libraries-syntax.vim) 语法高亮的知名的JS库
- [ultisnips](https://github.com/SirVer/ultisnips) 模板补全插件
- [vim-protodef](https://github.com/derekwyatt/vim-protodef) 根据类声明自动生成类实现的代码框架

</details>

## 错误处理

```
YouCompleteMe unavailable: dlopen(/usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload/_io.so, 2): Symbol not found:
__PyCodecInfo_GetIncrementalDecoder
  Referenced from: /usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload/_io.so
  Expected in: flat namespace
 in /usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload/_io.so
Press ENTER or type command to continue
```

## 参考资料

- [vim 大冒险：在游戏中学习 vim](http://vim-adventures.com/)
- [VimScript学会如何自定义Vim编辑器](http://learnvimscriptthehardway.onefloweroneworld.com/)
- [一起来说 Vim 语](http://www.jianshu.com/p/a361ce8c97bc)
- [css-color stopped working after updating Vim to 7.4](https://github.com/ap/vim-css-color/issues/29)
- [我的VIM配置及说明【K-VIM】](http://www.wklken.me/posts/2013/06/11/linux-my-vim.html)
- [简明 VIM 练级攻略](http://coolshell.cn/articles/5426.html)
- [Vi中的正则表达式](http://tech.idv2.com/2008/07/08/vim-regexp/)
- [vi替换字符串（zz）](http://blog.csdn.net/aldenphy/article/details/4019486)

## 其它人的vimrc配置

- [luofei614/vim-plug](https://github.com/luofei614/vim-plug/blob/master/.vimrc)
- [yangyangwithgnu/use_vim_as_ide](https://github.com/yangyangwithgnu/use_vim_as_ide/blob/master/.vimrc)
- [fingertap/vimrc](https://github.com/fingertap/vimrc/blob/master/.vimrc)
- [barretlee/autoconfig-mac-vimrc](https://github.com/barretlee/autoconfig-mac-vimrc/blob/master/.vimrc)
