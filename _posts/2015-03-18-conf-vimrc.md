---
author: Xiaojie Yuan
layout: post
title: "[Conf] .vimrc"
date: 2015-03-18
---

```vim
" === Vundle Options ===
set nocompatible
filetype off

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" let Vundle manage Vundle
Plugin 'gmarik/Vundle.vim'
" plugin from http://vim-scripts.org/vim/scripts.html
Plugin 'taglist.vim'
Plugin 'gtags.vim'
" plugin from github repo
Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'
Plugin 'scrooloose/nerdtree'
Plugin 'bling/vim-airline'
" iOS development specific
"Plugin 'msanders/snipmate.vim'
"Plugin 'Rip-Rip/clang_complete'
"Plugin 'guns/ultisnips'
"Plugin 'terhechte/syntastic'
"Plugin 'b4winckler/vim-objc'
"Plugin 'eraserhd/vim-ios.git'
call vundle#end()

" === General Options ===
let mapleader=" "
syntax on
set encoding=utf-8
set fileencodings=utf-8
set autoindent
set smartindent
set number
set ruler
set hlsearch
set incsearch
set t_Co=256
set laststatus=2
colorscheme default
hi Search cterm=underline ctermfg=yellow ctermbg=none
hi Visual cterm=underline ctermfg=yellow ctermbg=none
hi Pmenu cterm=none ctermfg=yellow ctermbg=darkblue
hi PmenuSel cterm=none ctermfg=darkblue ctermbg=yellow

" === Tab/Space Options ===
set tabstop=8
set shiftwidth=8
set softtabstop=8
"set expandtab

" === Fold Options ===
set foldenable
set foldmethod=syntax
set foldcolumn=0
"set foldlevel=99
hi Folded ctermbg=none
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zC' : 'zO')<CR>
" refer https://upload.wikimedia.org/wikipedia/en/1/15/Xterm_256color_chart.svg for color-number mappings
"if has("autocmd")
"    au BufWinLeave * mkview
"    au BufWinEnter * silent loadview
"    au! BufReadPost,BufWritePost * silent loadview
"endif

" === Gtags Options ===
map <C-n> :cn<CR>
map <C-p> :cp<CR>
map <C-\> :GtagsCursor<CR>
map <C-l> q:k<CR>

" === Taglist Options ===
map <F9> :TlistToggle<CR>
let TagList_Show_One_File=1
let Tlist_Auto_Open=0
let Tlist_Auto_Update=1
let Tlist_Exit_OnlyWindow=1
let Tlist_GainFocus_On_ToggleOpen=1
let Tlist_File_Fold_Auto_Close=1
let Tlist_Enable_Fold_Colum=1

" === Airline Options ===
let g:airline_theme='dark'
let g:airline#extensions#whitespace#mixed_indent_algo=1

" === Mouse Options ===
set mouse=r
set mousehide

" === Quickfix Options ===
"nnoremap <C-q> :cclose<CR> - use togglelist.vim ?
au FileType qf call AdjustWindowHeight(3, 6)
function! AdjustWindowHeight(minheight, maxheight)
  exe max([min([line("$"), a:maxheight]), a:minheight]) . "wincmd _"
endfunction

" === Clang Complete Options ===
" Disable auto completion, always <C-X> <C-O> to complete
"let g:clang_complete_auto=1
"let g:clang_use_library=1
"let g:clang_periodic_quickfix=0
"let g:clang_close_preview=1
" For Objective-C, this needs to be active, otherwise multi-parameter
" methods won't be completed correctly
"let g:clang_snippets=1
" Snipmate does not work anymore, ultisnips is the recommended plugin
"let g:clang_snippets_engine='ultisnips'
" This might change depending on your installation
"let g:clang_exec='/usr/bin/clang'
"let g:clang_library_path="/usr/local/Cellar/llvm/HEAD/lib/libclang.dylib"
"let g:clang_library_path="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/"

" === Syntastic Options ===
" Show sidebar signs.
"let g:syntastic_enable_signs=1
" Read the clang complete file
"let g:syntastic_objc_config_file = '.clang_complete'
" Status line configuration
"set statusline+=%#warningmsg#  " Add Error ruler.
"set statusline+=%{SyntasticStatuslineFlag()}
"set statusline+=%*
"nnoremap <silent> ` :Errors<CR>
" Tell it to use clang instead of gcc
"let g:syntastic_objc_checker = 'clang'

" === AutoCmd Options ===
if has("autocmd")
    filetype plugin indent on
endif

if has("autocmd")
    au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif

" === Misc Options ===
" Open files in the same directory as the current file
map ,e :e <C-R>=expand("%:h") . "/" <CR>
map ,t :tabe <C-R>=expand("%:h") . "/" <CR>
map ,s :split <C-R>=expand("%:h") . "/" <CR>
map ,v :vsplit <C-R>=expand("%:h") . "/" <CR>
" Map 'jj' to Escape key
inoremap jj <ESC>

" === Resize Options ===
nnoremap <silent> <Leader>- :exe "resize " . (winheight(0) - 5)<CR>
nnoremap <silent> <Leader>= :exe "resize " . (winheight(0) + 5)<CR>
nnoremap <silent> <Leader>9 :exe "vertical resize " . (winwidth(0) - 5)<CR>
nnoremap <silent> <Leader>0 :exe "vertical resize " . (winwidth(0) + 5)<CR>
```