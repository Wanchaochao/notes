if v:lang =~ "utf8$" || v:lang =~ "UTF-8$"
   set fileencodings=ucs-bom,utf-8,latin1
endif

set nocompatible	" Use Vim defaults (much better!)
set bs=indent,eol,start		" allow backspacing over everything in insert mode
"set ai			" always set autoindenting on
"set backup		" keep a backup file
set viminfo='20,\"50	" read/write a .viminfo file, don't store more
			" than 50 lines of registers
set history=50		" keep 50 lines of command line history
set ruler		" show the cursor position all the time

" Only do this part when compiled with support for autocommands
if has("autocmd")
  augroup redhat
  autocmd!
  " In text files, always limit the width of text to 78 characters
  autocmd BufRead *.txt set tw=78
  " When editing a file, always jump to the last cursor position
  autocmd BufReadPost *
  \ if line("'\"") > 0 && line ("'\"") <= line("$") |
  \   exe "normal! g'\"" |
  \ endif
  " don't write swapfile on most commonly used directories for NFS mounts or USB sticks
  autocmd BufNewFile,BufReadPre /media/*,/mnt/* set directory=~/tmp,/var/tmp,/tmp
  " start with spec file template
  autocmd BufNewFile *.spec 0r /usr/share/vim/vimfiles/template.spec
  augroup END
endif

if has("cscope") && filereadable("/usr/bin/cscope")
   set csprg=/usr/bin/cscope
   set csto=0
   set cst
   set nocsverb
   " add any database in current directory
   if filereadable("cscope.out")
      cs add cscope.out
   " else add database pointed to by environment
   elseif $CSCOPE_DB != ""
      cs add $CSCOPE_DB
   endif
   set csverb
endif

" Switch syntax highlighting on, when the terminal has colors
" Also switch on highlighting the last used search pattern.
if &t_Co > 2 || has("gui_running")
  syntax on
  set hlsearch
endif

filetype plugin on

if &term=="xterm"
     set t_Co=8
     set t_Sb=[4%dm
     set t_Sf=[3%dm
endif

" Don't wake up system with blinking cursor:
" http://www.linuxpowertop.org/known.php
let &guicursor = &guicursor . ",a:blinkon0"

set mouse=a
" 不要使用vi的键盘模式，而是vim自己的 
set nocompatible 
"设置编码
set encoding=utf-8
set fileencoding=utf-8
"设定文件编码类型，彻底解决中文编码问题
set fenc=utf-8 
set fencs=utf-8,usc-bom,euc-jp,gb18030,gbk,gb2312,cp936
"显示行号
set number
"语法高亮
syntax on
"背景颜色
"set background=dark
"自动对齐
set autoindent
"对齐格式
set smartindent
"TAB为4个空格
set tabstop=4
set shiftwidth=4
"括号匹配
set showmatch
"去掉utf-8 Bom
set nobomb
"history文件中需要记录的行数 
set history=100
" 侦测文件类型 
filetype on 
" 载入文件类型插件 
filetype plugin on 
" 为特定文件类型载入相关缩进文件 
filetype indent on
"配色方案
"Solarized"
let g:solarized_termcolors=256
set t_Co=256
set background=dark
colorscheme lucius
"状态栏
set laststatus=2
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff},%{&encoding}]\ [TYPE=%Y]\ [POS=%l,%v]\ [%p%%]\ %{strftime(\"%y/%m/%d\ -\ %H:%M\")}
"关闭启动提示
set shortmess=atI
"高亮当前行
set cursorline
"set cursorColumn
"防止特殊符号无法正常显示
set ambiwidth=double
"防止粘贴缩进混乱
set paste
