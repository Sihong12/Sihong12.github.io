---
layout: post
title: Vim 설정하기
redirect_to:
- https://hongsii.github.io/2018/01/29/vim_configuration/
date: '2018-01-29 00:41'
tags:
  - vim
  - 에디터
categories:
  - Programming
---

# Vim 설정하기
처음 vim을 사용할 때에 기본적으로 지원되지 않는 것들이 있습니다.
라인 넘버가 안보이는 경우 vim 안에서 `:set number`라고 입력하면 라인 넘버가 보이지만, vim을 종료하게 되면 해당 옵션이 유지되지 않습니다.
설정을 유지하기 위해선 설정파일에 필요한 옵션을 설정해줄 필요가 있습니다.

기본적으로 설정파일이라 숨김파일로 홈폴더 밑에 존재하며 파일의 위치는 `~/.vimrc` 입니다.
해당 위치에 존재하지 않는 경우는 동일한 파일명으로 생성해주면 됩니다.

on, off 가능한 옵션은 **toggle 명령어**를 사용할 수 있습니다.
toggle 명령어는 `set [옵션]!` 또는 `set inv[옵션]`로 사용합니다.
toggle 명령어를 사용하게 되면 현재 적용되고 있는 옵션의 반대 옵션을 적용시킵니다.

toggle을 이용하지 않고 옵션을 끄고 싶다면 반대 옵션을 사용하면 됩니다.
반대 옵션은 보통 `no[옵션]`으로 되어 있습니다. 예를 들어, `hlsearch`라는 옵션이 있을 때, 반대 옵션은 `nohlsearch`가 됩니다.
옵션을 줄여서 사용할 때도 동일한 방법으로 반대 옵션을 적용할 수 있습니다.

## 화면 설정

``` vim
syntax on " 형식별 구문 강조 표시
set number " 라인 넘버 표시. (= nu)
set showcmd " 사용자가 입력한 명령어 표시
set showmatch " 현재 선택된 괄호의 쌍을 표시
set relativenumber " 커서를 기준으로 라인 넘버 표시. 커서 위치에 따라 바뀜. (= rnu)
set cursorline " 커서가 있는 라인을 강조 표시. (= cul)
set colorschme [scheme명] " 테마 적용.
set ruler " 커서 위치 표시. (= ru)
set laststatus=2 " 상태바 표시. (= ls) [0: 상태바 미표시 / 1: 2개 이상의 윈도우에서 표시 / 2: 항상 표시]
" 상태바 커스터마이징 %<item>으로 사용하며, \는 구분자로 공백을 넣을 경우는 구분자를 넣어줘야함.
set statusline=%F\ %y%m%r\ %=Line:\ %l/%L\ [%p%%]\ Col:%c\ Buf:%n
hi statusline ctermfg=White ctermbg=4 cterm=none "활성화된 상태바 배경색 및 폰트색 설정
hi statuslineNC ctermfg=White ctermbg=8 cterm=none " 윈도우가 2개 이상인 경우 비활성화된 윈도우의 배경색 및 폰트색 설정
set mouse=a " 마우스로 스크롤 및 리사이즈 가능. [n : Normal mode / v : Visual mode / i : Insert mode / a : All modes]
```

## 검색 설정

``` vim
set hlsearch " 검색된 결과 강조 표시. (= hls)
set ignorecase " 검색시 대소문자를 구분하지 않음. (= ic)
set incsearch " 검색어를 입력할 때마다 일치하는 문자열을 강조해서 표시. (= is)
set smartcase " ignore 옵션이 켜져있더라도 검색어에 대문자가 있다면 정확히 일치하는 문자열을 찾음. (= scs)
```

## 들여쓰기 설정

``` vim
set autoindent " 새로운 라인이 추가될 때, 이전 라인의 들여쓰기에 자동으로 맞춤. (= ai)
set expandtab  " Tab을 Space로 변경. (= et)
set tabstop=4 " 탭으로 들여쓰기시 사용할 스페이스바 개수. (= ts)
set shiftwidth=4 " <<, >> 으로 들여쓰기시 사용할 스페이스바 개수. (= sw)
set softtabstop=4 " 스페이스바 n개를 하나의 탭으로 처리. (= sts)
" ex) 스페이스바 4개가 연속으로 있다면 백스페이스로 스페이스바를 지우면 스페이스바 4개를 하나의 탭으로 인식해 삭제.
filetype indent on " indent.vim 파일에 설정된 파일 형식별 들여쓰기 적용.
```

## 입력 설정

``` vim
set clipboard=unnamed " vim에서 복사한 내용이 클립보드에 저장
set backspace=eol,start,indent " 라인의 시작과 끝의 들여쓰기를 백스페이스로 지움.
set history=1000 " 편집한 내용 저장 개수 (되돌리기 제한 설정)
set paste " 다른 곳에서 복사한 내용을 붙여넣을 때, 자동 들여쓰기가 적용되는 것을 막아 복사한 내용을 들여쓰기없이 복사.
set pastetoggle=<F2> " paste 옵션이 적용되면 들여쓰기가 옵션이 제대로 작동하지 않기 때문에 toggle식으로 옵션을 키고 끌 수 있음.
```
clipboard 옵션은 vim에서 복사한 내용을 클립보드에 저장해 다른 어플리케이션에서도 사용할 수 있게 해줍니다.<br/>
해당 옵션을 설정하면 OS별로 클립보드로 사용하는 레지스터에 값을 추가로 저장해줍니다.

옵션은 `unnamed`, `unnamedplus`(>= vim7.4) 두 개가 있으며 `unnamed`로 설정하면 `*`레지스터에 추가로 저장해주고, `unnamedplus`로 설정하면 `+` 레지스터에 추가로 저장됩니다.
해당 옵션을 설정하기 위해 OS별로 어떤 레지스터를 사용하는지 알아야 합니다. <br/>
OS별 클립보드로 사용하는 레지스터는 아래와 같습니다.
* Windows : `+`레지스터 = `*`레지스터 (Windows에서 두 레지스터는 동일하게 사용)
* OSX : `*`레지스터
* Linux : `+`레지스터(Ctrl+C, Ctrl+V), `*`레지스터(drag, mouse button)

Windows와 OSX는 모두 하나의 클립보드를 이용하며, Windows에서는 `*`, `+` 두 레지스터 모두 클립보드로 사용됩니다.  Linux에서는 앞의 두 OS와 다르게 두 개의 클립보드가 있습니다. Ctrl+C, Ctrl+V를 사용하면 `+`레지스터를 이용하고(CLIPBOARD), 마우스로 드래그해서 선택하여 마우스로 버튼으로 복사할 때는 `*`레지스터를 사용합니다(PRIMARY).

OS별로 아래와 같이 설정하면 vim에서 복사한 내용을 다른 곳에서 Ctrl+C, Ctrl+V 를 이용해 복사할 수 있습니다.

> Windows, OSX : `set clipboard=unnamed` <br/>
> Linux : `set clipboard=unnamedplus`

더 많은 옵션들이 있지만, 자주 쓸만한 옵션만 정리해보았습니다.
<br/>

## 플러그인

vim에서 플러그인을 사용하려면 [Vundle](https://github.com/VundleVim/Vundle.vim)이라는 플러그인 매니저가 필요합니다.<br/>
설치 방법 및 유용한 플러그인까지 알아보겠습니다.

### Vundle 설치

Vundle의 github을 clone받으면 필요한 파일을 받을 수 있습니다.<br/>
아래 명령어를 터미널창에서 입력해줍니다.

> git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

`~/.vim/bundle` 폴더 안에 플러그인 매니저 폴더(Vundle.vim)가 생기고 필요한 파일이 다운로드 됩니다. 앞으로 플러그인을 추가하게 되면 bundle폴더에 파일이 추가됩니다.

### vimrc 설정

이제 필요한 파일은 준비가 되었으니 플러그인 매니저 사용을 위한 옵션을 설정해줍니다.

``` vim
set nocompatible " Vim으로 사용한다는 뜻
filetype off " 필수 옵션

set rtp+=~/.vim/bundle/Vundle.vim " runtimepath 설정
call vundle#begin()
" 필요한 플러그인 추가
" Plugin 'github repository'
Plugin 'VundleVim/Vundle.vim'
call vundle#end()

filetype plugin indent on
```
<br/>
옵션 중 `filetype off`한 뒤, 다시 켜는 이유는 vim은 runtimepath의 플러그인을 캐싱하는데 Vundle은 runtimepath를 변경하기 때문에 꺼두었다가 작업이 끝나면 켜지도록 해야합니다.

### 플러그인 설치

설정이 끝나면 설정파일 저장 후, vim을 다시 켜거나 `:source %`를 입력해 설정 파일을 다시 로드해줍니다.<br/>
이제 명령모드에서 `:PluginInstall`을 입력하면 설정 파일에 추가한 플러그인을 설치합니다.
좌측 하단에 Done! 이라는 문구가 표시되면 설치가 완료됩니다.
![플러그인 설치 완료]({{ site.url }}/assets/images/post_image/2018/01/vim_plugin_install_success.png)

### 플러그인 삭제

플러그인을 삭제하고 싶다면 **설정 파일에서 추가한 플러그인의 git repository를 삭제**하고 설정 파일을 다시 로드한 다음 `:PluginClean`을 입력해주면
설치할 때와 동일하게 좌측에 새창이 뜨면서 삭제할 플러그인이 보이고 삭제 여부를 물은 다음에 삭제 과정이 진행됩니다.
<br/>

## 유용한 플러그인
다양한 플러그인에 대한 정보가 필요하다면 [Vim Awesome](https://vimawesome.com/)에서 찾을 수 있습니다.<br/>
플러그인을 추가하려면 직접 git을 clone받아서 설정하거나, Vundle을 이용해 설정하는 방법이 있습니다. 위에서 Vundle을 설치했기 때문에 해당 설치 방법만 설명하겠습니다.

### The NERD Tree
[The NERD Tree](https://github.com/scrooloose/nerdtree)는 vim내에서 Tree 형태로 폴더 구조를 보여주는 플러그인입니다.<br/>
먼저 `vimrc`에 **NERD Tree의 git repository를 추가**합니다.

``` vim
call vundle#begin()
" 필요한 플러그인 추가
" Plugin 'github repository'
Plugin 'VundleVim/Vundle.vim'
Plugin 'scrooloose/nerdtree' " NERD Tree 추가
call vundle#end()
```
설정 파일을 다시 로드 후, vim의 명령모드에서 `:PluginInstall`을 입력하면 설치가 완료됩니다. <br/>
NERD Tree 실행은 명령모드에서 `:NERDTree`라고 입력하면 좌측에 Tree view가 표시됩니다.

매번 명령어를 입력하기 귀찮기 때문에 단축키 설정을 해줍니다. <br/>
설정 파일을 연다음에 아래와 같이 설정해줍니다. 전 단축키를 `\nt`로 설정하겠습니다.<br/>
단축키 설정은 `map 단축키 명령어`로 합니다.
``` vim
let mapleader="\\"
map <Leader>nt <ESC>:NERDTree<CR>
```
이제 NERD Tree의 추가 옵션을 설정해보겠습니다.

``` vim
" Tree 아이콘 변경
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
" 파일없이 vim만 틸 경우 자동으로 NERD Tree 실행.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
" 디렉토리를 vim으로 여는 경우 NERD Tree 실행.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif
```

## My vimrc
아래는 제가 사용 중인 설정입니다.

``` vim
set nocompatible
filetype off
set rtp+=~/.vim/bundle/Vundle.vim

call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'scrooloose/nerdtree'
"Plugin 'vim-airline/vim-airline'
"Plugin 'vim-airline/vim-airline-themes'
Plugin 'airblade/vim-gitgutter'
Plugin 'tpope/vim-fugitive'
Plugin 'ctrlpvim/ctrlp.vim'
"Plugin 'Lokaltog/vim-powerline'
call vundle#end()

filetype plugin indent on

set number " 라인 넘버 표시. (= nu)
set showcmd " 사용자가 입력한 명령어 표시
set showmatch " 현재 선택된 괄호의 쌍을 표시
"set relativenumber " 커서를 기준으로 라인 넘버 표시. 커서 위치에 따라 바뀜. (= rnu)
"set cursorline " 커서가 있는 라인을 강조 표시. (= cul)
set ruler " 커서 위치 표시. (= ru)
set clipboard=unnamed " 복사시 추가로 클립보드에 저장
set mouse=a " 마우스로 스크롤 및 리사이즈 가능. [n : Normal mode / v : Visual mode / i : Insert mode / a : All modes]
set laststatus=2 " 상태바 표시. (= ls) [0: 상태바 미표시 / 1: 2개 이상의 윈도우에서 표시 / 2: 항상 표시]
set statusline=%F\ %y%m%r\ %=Line:\ %l/%L\ [%p%%]\ Col:%c\ Buf:%n " 상태바 커스터마이징 %<item>으로 사용하며, \는 구분자로 공백을 넣을 경우는 구분자를 넣어줘야함.
hi statusline ctermfg=White ctermbg=4 cterm=none "활성화된 상태바 배경색 및 폰트색 설정
hi statuslineNC ctermfg=White ctermbg=8 cterm=none " 윈도우가 2개 이상인 경우 비활성화된 윈도우의 배경색 및 폰트색 설정

" Searching options
set hlsearch " 검색된 결과 강조 표시. (= hls) <-> nohlsearch (= nohls)
set ignorecase " 검색시 대소문자를 구분하지 않음. (= ic) <-> noignorecase (= noic)
set incsearch " 검색어를 입력할 때마다 일치하는 문자열을 강조해서 표시. (= is) <-> noincsearch (= nois)
set smartcase " 검색어에 대문자가 있다면 정확히 일치하는 문자열을 찾음. ignorecase 옵션이 on이어도 됨. (= scs) <-> nosmartcase (= noscs)
syntax on " 형식별 구문 강조 표시

" Indentation options
set autoindent " 새로운 라인이 추가될 때, 이전 라인의 들여쓰기에 자동으로 맞춤. (= ai)
set expandtab  " Tab을 Space로 변경. (= et)
set tabstop=4 " 탭으로 들여쓰기시 사용할 스페이스바 개수. (= ts)
set shiftwidth=4 " <<, >> 으로 들여쓰기시 사용할 스페이스바 개수. (= sw)
set softtabstop=4 " 스페이스바 n개를 하나의 탭으로 처리. (= sts)
" ex) 스페이스바 4개가 연속으로 있다면 백스페이스로 스페이스바를 지우면 스페이스바 4개를 하나의 탭으로 인식해 삭제.

" Input options
set backspace=eol,start,indent " 라인의 시작과 끝의 들여쓰기를 백스페이스로 지움.
set history=1000 " 편집한 내용 저장 개수 (되돌리기 제한 설정)

" Key settings
let mapleader="\\"

" Plugin configuration
"" The NERD Tree
map <Leader>nt <ESC>:NERDTree<CR>

autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif

autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif

"" Ctrlp
let g:ctrlp_map = '<c-p>'
let g:ctrlp_cmd = 'CtrlP'

set wildignore+=*/tmp/*,*.so,*.swp,*.zip " MacOSX/Linux
let g:ctrlp_custom_ignore = {
  \ 'dir':  '\v[\/]\.(git|hg|svn)$',
  \ 'file': '\v\.(exe|so|dll)$',
  \ }

"" Vim-airline
let g:airline#extensions#tabline#enabled = 1
let g:airline_powerline_fonts = 1
```
--------------------------------
# 참고
* [Top 50 Vim Configuration Options](https://www.shortcutfoo.com/blog/top-50-vim-configuration-options/)
