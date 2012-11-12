Setting and Querying Options in Vim
===================================

<http://usevim.com/2012/11/09/vim-101-options/>

`:help set` (which also contains a list of summaries of all options in Vim).

Setting Options
---------------

`:set option=value` Set an option to a value.  
`:set option+=value` Append to a string option or add to a number option.  
`:set option-=value` Remove from a string option or subtract from a number
option.

`:set option&` Reset an option to its default value.  
`:set all&` Reset all options.

### Boolean Options

`:set spell` Turn `spell` on.  
`:set nospell` Turns `spell` off.

Querying Options
----------------

`:set` Show all options that differ from their default values.
`:set all` Show all options.
`:set option?` Show the value of a particular option.
