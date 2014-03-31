Put this in your vimrc:

    nnoremap <silent> <C-l> :<C-u>nohlsearch<CR><C-l>

ctrl-l normally tells vim to redraw the screen, now it redraws the screen and
clears the search highlights. No more doing `/zzz` searches to clear the search
highlights. Read in
[Practical Vim](http://pragprog.com/book/dnvim/practical-vim).
