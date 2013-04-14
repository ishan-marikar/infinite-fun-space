Smart Search with Subvert
=========================

Notes from <http://vimcasts.org/episodes/smart-search-with-subvert/>

Also see `:help :Subvert`.

The `:Subvert` (or `:S`) command is a replacement for the substitute (`%s`)
command, that lets you do certain kinds of regular expression searches and
substitutions using a much easier and more intuitive pattern language. It comes
from Tim Pope's [abolish.vim](https://github.com/tpope/vim-abolish) plugin.

`:S/pumpkin` does a search that matches `PUMPKIN`, `Pumpkin` or `pumpkin`.

`:S/m{ouse,ice}` matches `MOUSE`, `Mouse`, `mouse`, `MICE`, `Mice` or `mice`.

`:S/{pumpkin,mouse,user}` matches `PUMPKIN`, `Pumpkin`, `pumpkin`, `MOUSE`,
`Mouse`, `mouse`, `USER`, `User` and `user`.

`:S/insert_mode` matches both `insert_mode` and `InsertMode` (search words with
underscores in them automatically match their CamelCase equivalents).

Typing `/<C-r>/` will paste the last search pattern used (from the search
register) into the command line, so you can see what Subvert actually searched
for.


Search across multiple files
----------------------------

`:S/package_create/ **/*.py` searches for `package_create` in all `*.py` files
in Vim's current working directory and below. The matches are placed into the
quickfix list, do `:copen` to get a list of them.
