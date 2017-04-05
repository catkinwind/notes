* Let vim recognize markdown file

File type rules are defined in /usr/share/vim/vim74/filetype.vim. We could override it in ~/.vimrc file.

autocmd BufNewFile,BufRead \*.md set filetype=markdown
