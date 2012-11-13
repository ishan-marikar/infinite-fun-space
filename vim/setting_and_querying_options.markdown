Setting and Querying Configuration Options in Vim
=================================================

Inspired by [usevim.com's post](http://usevim.com/2012/11/09/vim-101-options/) I decided to read
`:help set` (which also contains a list of summaries of all options in Vim).

Setting and Resetting Options
-----------------------------

*  `:set option=value` Set the value of an optio.  
*  `:set option+=value` Append to a string option or add to a number option.  
*  `:set option-=value` Remove from a string option or subtract from a number
   option.
*  `:set option&` Reset an option to its default value.  
*  `:set all&` Reset all options.

### Boolean Options

*  `:set spell` Turn `spell` on.  
*  `:set nospell` Turns `spell` off.

Querying Options
----------------

*  `:set` Show all options that differ from their default values.
*  `:set all` Show all options.
*  `:set option?` Show the value of a particular option.
