Setting and Querying Options in Vim
===================================

<http://usevim.com/2012/11/09/vim-101-options/>

`:help set` (which also contains a list of summaries of all options in Vim).

Setting Options
---------------

`:set option=value` sets an option to a value.
`:set option+=value` appends to a string option or adds to a number option.
`:set option-=value` removes froma a string option or subtracts from a number
option.

`:set option&` resets an option to its default value.
`:set all&` resets all options.

### Boolean Options

`:set spell` turns `spell` on.  
`:set nospell` turns `spell` off.

Querying Options
----------------

`:set`
:   Show all options that differ from their default values.

`:set all`
:   Show all options.

`:set option?`
:   Show the value of a particular option.
