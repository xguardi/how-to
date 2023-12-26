# VIM cheatsheet

## Files 

<!-- <leader>n          Rename (and reopen) current file -->

## Fuzzy File Finder fzf

C-p                 File finder
C-k                 Move to next item from matchlist
C-j                 Move to previous item from matchlist
C-x                 Open the selected file in horizontal split
C-v                 Open the selected file in vertical split
C-t                 Open the selected file in a new tab page
C-c                 Dismiss fzf

:Rg                 Launches the grep (ripgrep) on the current folder

:Files              Finds in files
:Buffers            Finds in buffers
:Tags               Finds in tags
:Snippets           Finds in snippets
:Maps               Finds in mappings
:Helptags           Finds in help
:Colors             Lists color schemes
:Marks              Lists all marks

## Buffers

C-6                 Switch to previous buffer

## Tabs

tab ball            Show each buffer in a tab (up to 'tabpagemax' tabs)
gt                  Go to next tap
gT                  Go to previous tab
{i}gt               Go to tab in position i
tabm {i}            Move current tab to position i+1

## Windows

C-W =               Resize windows to equal height/width:

C-W >               Increase window width
C-W <               Decrease window width

C-W +               Increase window height
C-W -               Decrease window height

C-W J               Switch vertical splits to horizontal
C-W H               Switch horizontal splits to vertical

## Sessions

mksession!          Records current session in a Session.vim file
$ vim -S            Restores session
source Session.vim  Restores session

## Folding & Numbering

za / <space>        	Toogle folding
zR                  	Open all folds
zM			Close all folds
zi			Toggle 'foldenable'

set number		    Enable line numbering
set rnu			    Enable line relative numbering
set nonumber		Disable line numbering
set nornu		    Disable line relative numbering
set nofoldable      Disable folding

## Comments (tpope/vim-commentary)

gcc			Comment out a line
gcap			Comment out a pragraph

## Encodings

echo &fenc          Show file encoding

## Mappings

nmap			List all normal mode mappings
vmap			List all visual mode mappings
imap			Lost all insert mode mappings

map <Leader>m		Inspect mapping for <Leader>m
map <F5>		Inspect mapping for <F5>
map j			Inspect mapping for j

nmap x ddx		Evil recursive loop
nnoremap x ddx		Valid mapping that avoids recursion!

nonrecursive mapping variants noremap should be used ALWAYS.

## Git (fugitive plugin)

Git 			Run any arbritary git command
Gwrite			Stage the current file to the index
Gread			Revert current file to last checked in version
Gremove			Delete the current file and the corresponding Vim buffer
Gmove			Rename the current file and the corresponding Vim buffer
Gcommit 		Opens up a commit window in a split window
Gblame 			Opens a vertically split window containing annotations for each line of the file

Gstatus			Opens the status window

On the status window:

C-n			Jumps to the next file
C-p			Jumps to the previous file
-			Adds/resets a file to the index

Gdiff			Comparing working copy with index version

## REPL - Slime (Python / R / Lisp / ...)

C-c C-c			Selects current paragraph and send it to the REPL. 
			In visual mode only send the selected text.

C-c v			Repromt session and window name

## Editing

Option + Command	Let you select a region to copy to clipboard. Useful when linenumbers are on display.

cit			[C]hange [i]n [t]ag useful when editing html.
dit			[D]elete [i]n [t]ag useful when editing html.

gU			Switch to Uppercase.
gu			Switch to Lowercase.

set paste
set nopaste

## Vim help system

helpgrep something	Open s quickfix window with grep matches

C-]			Jump into the tag
C-t			Come back from the tag

## Spell

set spell		Enable spellchecking
set nospell		Disable spellchecking

set spell spelllanguage=ca,en_us	 Languages should already been predefined at the vimrc, but if not, simply:

`]s`			move to the NEXT misspelled word
`[s`			move to the PREVIOUS misspelled word

z= 			List of suggested alternatives.

zg 			Add word to dictionary

This adds the word to a spellfile located by default at .config/nvim/spell where all the added words are stored and are loaded together with the main dictionary. It's a ood idea to keep this (actually the whole nvim folder) in same repo to never lose this work.
Misspelled words are highlighted. If you are not comfortable with the default highlight colors you can change them:

hi clear SpellBad
hi SpellBad cterm=underline

## Scripting

scriptnames		Lists all the scripts in order as they are sourced during startup

## Word counting

gC-g			Count words from a block of text.

## Moving around

Move through the jump list:

C-O (back) C-I (fwd)

Show jump list:
  
ju

Moving through the change list:

  g; (back) g, (fwd)

Show changes list:

  :changes

## Editing

Paste from outside vim without autoindenting

  :set paste
  :set nopaste

## Surround

Surround whole line in php tags 

  yss-

## Browser

Open url in default browser

:gx 

Open current file in browser

,b

## Python REPL - iron.vim

Open a REPL for current file type
:IronRepl

Send a chunk of text to REPL. Works with motions
ctr

## Editing HTML files

Sends the current html file to the browser
<leader>b

## Shell commands (To be copied to it own file soon)

$ tree        Show all files underneath the current directory in a tree-like view

-- Old file --

## spell -----

http://cfenoy.github.io/blog/2014/01/23/vim-spellcheck/

---------------------------
#  Navigating with ctags  #
---------------------------

By default, Mac OSX somes with a BSD flavour of ctags. It's better to install Exhuberant CTags with:

$ brew install ctags

From the command line indexing a whole folder is something as simple as:

> ctags -R .

This generates a tags file with the index.

To tell vim where to find the tags file we need to set the path to it:

:set tags=tags,./tags,./.tags

We can then manually execute ctags to generate the index for our current project by running:

:!ctags -R

We can force to rerun ctags every time we save a file by:

:autocmd BufWritePost * call system("ctags -R")

However, a neater approach is to add a hook to your git global configuration so that is reruns ctags 
everytime we commit. All the details are explained at https://tbaggery.com/2011/08/08/effortless-ctags-with-git.html

In a nutshell:

$ git config --global init.templatedir '~/.git_template'
$ mkdir -p ~/.git_template/hooks
Inside this last folder create the following executable:
    #!/bin/sh
    set -e
    PATH="/usr/local/bin:$PATH"
    dir="`git rev-parse --git-dir`"
    trap 'rm -f "$dir/$$.tags"' EXIT
    git ls-files | \
      ctags --tag-relative -L - -f"$dir/$$.tags"
    mv "$dir/$$.tags" "$dir/tags"

This will create a .git/tags file anytime we commit something to the repo. 
We then need to create three execs hooks (post-commit, post-merge and post-checkout) at ~/.git_template/hooks with:
    #!/bin/sh
    .git/hooks/ctags >/dev/null 2>&1 &
And a fourth (post-rewrite) with:
    #!/bin/sh
    case "$1" in
      rebase) exec .git/hooks/post-merge ;;
    esac
Then we can reload them in our git project (if it already exists) by running:
$ git init
This will copy the hooks inside .git.

Let's see how we can use the tags.

Jump to a tag by looking up the name
:tag name

If we have a fuzzy searcher such as fzf already installed we can use it to search through the tags:
:Tags (or the remapped <leader>t)  

Using the CTRL-] command you can jump to a tag under your cursor.

Using the tag stack, which is a list of tag-based locations you have visited. After following some tags, you can trace back through your history with :pop or CTRL-T and forward with :tag (each take an optional count). To see your history, you can use :tags. This works much like Vim’s regular CTRL-I and CTRL-O commands for the jump list, but the tag stack only contains, well, tags.

Tagbar is a Vim plugin that provides an easy way to browse the tags of the current file and get an overview of its structure. It does this by creating a sidebar that displays the ctags-generated tags of the current file, ordered by their scope. This means that for example methods in C++ are displayed under the class they are defined in.
http://majutsushi.github.io/tagbar/

TODO: how to use tagbar
--------

TODO: Nerdtree
TODO: auto-complete
TODO: vim-notes https://github.com/xolox/vim-notes
TODO: password manager https://www.passwordstore.org/
