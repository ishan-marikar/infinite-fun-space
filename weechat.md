WeeChat
=======

WeeChat is much better than irssi:

+ Nicer default colours 
+ Has a scrollable user list on the right-hand-side by default
+ You can easily get a list of joined channels on the left-hand-side as well
+ Shows away message in status line
  (you can see when you're away instead of just forgetting!)
+ Much easier to configure, and proper documentation of all the config options
+ Built-in command for finding and installing scripts


<http://www.weechat.org/files/doc/stable/weechat_user.en.html>


Installing
----------

Ubuntu 12.04's default version of weechat is quite old, so install their PPA:

    sudo add-apt-repository ppa:nesthib/weechat-stable

Then install weechat, making sure you get the curses UI and the plugins and
scripts:

    sudo apt-get install weechat weechat-curses weechat-plugins weechat-scripts

The command to run weechat is:

    weechat-curses


Quitting
--------

To get out of weechat type:

    /quit


Fixing the status and title bar colors (weechat configuration)
--------------------------------------------------------------

List all the bars:

    /bar

List all the options for the "status" bar:

    /set weechat.bar.status.*

One of the settings is `weechat.bar.status/color_bg = blue`, make this black
instead:

    /set weechat.bar.status.color_bg black

Now the status bar text is legible in the Solarized Dark terminal colour
scheme. Repeat for the title bar:

    /set weechat.bar.title.color_bg black

These settings will be automatically saved when you quit weechat (you can also
type `/save` to save them now).

Generally, use the `/set` command to view and set options, which will be
automatically saved (to `~/.weechat/weechat.conf`) when you quit weechat.

The `/unset` command resets options to their defaults.

The `iset` script lets you interactively view and set config options from
within a weechat buffee:

    /script install iset.pl
    /iset


### Customising the status bar

You can create a custom status bar with a command like this:

    /bar add mystatus window bottom 1 off [buffer_number+/+buffer_count],buffer_name+buffer_filter,[hotlist],completion,scroll

The details of the different status bar options and items in the command are in
the weechat docs. Delete the status bar with:

    /bar del mystatus

Then you can recreate it with a slightly tweaked `/bar add mystatus ...`
command, rinse and repeat until you've got it right.

Then set options such as the colours of the custom status bar:

    /set weechat.bar.mystatus.color_bg black

Once you have your custom status bar how you want it, delete the default status
bar:

    /bar del status


Connecting to IRC servers and joining channels (weechat buffers)
----------------------------------------------------------------

### Connect to an IRC Server

    /connect irc.freenode.net

List servers:

    /server

To automatically connect to the server when weechat starts from now on:

    /set irc.server.freenode.autoconnect on


### Join an IRC Channel

    /join #ubuntu

Each channel is opened in its own weechat _buffer_. Use `ctrl-n` and `ctrl-p`
to move between buffers (also F5 or alt + arrow keys). `/buffer` lists open
buffers (the list is printed to the "core" buffer, i.e. the first buffer).

To tell weechat to automatically join a list of channels whenever it connects
to a server, do:

    /set irc.server.freenode.autojoin ckan,okfn


### Identifying yourself

Tell weechat what nicks to use on a server, in order of preference:

    /set irc.server.freenode.nicks = "seanh,seanh_,seanh__"

To automatically identify yourself to the irc server after connecting, do:

    /set irc.server.freenode.command /msg NickServ identify <password>


Search
------

There's a really good incremental search through the channel history if you hit
`ctrl-r` and then start typing (press `ctrl-r` again for case-sensitive
matching).

Up and down arrows move between search matches.

Enter leaves search.


Mouse Support
-------------

    /set weechat.look.mouse on

You can use the scrollwheel to scroll through channel history or nick list,
click on a nickname to private message someone, and who knows what else.

Even works if weechat is running on a remote server over ssh!

Note: this _will_ interfere with copying text out of weechat using the mouse.


Going Away and Coming Back
--------------------------

    /away <reason>

Sets your status to away on the current IRC server.

    /away -all reason

Sets your status to away on all connect IRC servers.

    /away
    /away -all

Unsets your away status on the current, or all, IRC servers.


Splitting Windows
-----------------

A weechat _window_ is a screen area that displays a buffer. You can split the
screen into many windows.

    /window splith
    /window splitv

To move the cursor between windows do `alt+w, alt+arrow key`.


Check if someone is online
--------------------------

    /ison <nick>


Send someone a private message
------------------------------

    /msg <nick> <message>


Logging
-------

Content of buffers is automatically saved to files in `~/.weechat/logs` by
default. If you want to make sure, the option that controls this is:

    /set logger.file.auto_log on


Scripts
-------

Scripts add features to weechat and can be written in various languages
including Python, Perl and Ruby. The Ubuntu package `weechat-scripts` contains
a bunch of them, and more can be found at <http://www.weechat.org/scripts>.

To install a script, just do e.g.:

    /script install buffers.pl

The script will autoload next time you start weechat.

The foreground and background colours of the current buffer in the buffer list
clash with my terminal theme, to change them:

    /set plugins.var.perl.buffers.color_current black,gray

