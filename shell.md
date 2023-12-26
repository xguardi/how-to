# Shell cheatsheet

## Looking up space on disk

List file sizes in human format

`$ du -sh . | more`

List the top 20 largest files

`$ su -sh  . | sort -nr | head -n 20`

Check disk usage

`$ df`

## Fuzzy File Finder FZF

Search through your command history

`CTRL+R`

Fuzzy completion for bash/zsh

`$ vim notes/**`

Search the hostname to SSH into

`$ ssh **`

Kill a process fast

`$ kill -9 <TAB>`

