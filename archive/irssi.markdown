irssi
=====

<http://irssi.org/documentation/manual>

## Help

You can get help for an irssi command by doing:

    /help <command>

## Scrolling

`PgUp` and `PgDn` or `Meta-p` and `Meta-n` scroll up and down within the
current window.


## Windows

Irssi creates a new "hidden window" every time you join a channel or query/msg
a user.  There's also a "status window" where any messages that don't belong in
a channel or msg window go. The lack of discoverability of the window-swtiching
commands  is the most confusing thing about irssi at first.

To switch between windows:

`ctrl-n`, `ctrl-p`  Go to next, previous window.

`Alt-n` or `/window n`  Go directly to window number `n`.

`/window list`  Print the list of windows to the status window (window 1)


## Config Settings

Your irssi config file is at:

    ~/.irssi/config

The config file is not very human readable so it's easiest to edit your
settings from within irssi instead of editing the config file directly:

    /set

Prints all your current settings and their values to the status window.

    /set <key>

Prints settings that match `<key>`.

    /set <key> <value>

Sets setting `<key>` to `<value>`.

Tab-complete works for setting keys (and for other things in irssi, like
commands and server, channel and user names).

    /save

Save your current config settings to file.

    /reload

Reload your saved settings from file.


## Username

    /set nick seanh
    /set alternate_nick seanh
    /set user_name seanh
    /set real_name "Sean Hammond"
    /save


## IRC Networks & Servers

<http://pthree.org/2010/02/02/irssis-channel-network-server-and-connect-what-it-means/>

To get irssi to automatically connect to irc.freenode.net and identify
yourself, you have to define both an IRC network and a server on that network:

    /network add -nick seanh -user seanh -realname "Sean Hammond" -autosendcmd "/msg NickServe identify secret" freenode
    /server add -auto -network freenode irc.freenode.net

`/network` List all your configured networks in the status window.

`/server`   List servers you're connected to in the status window.

`/server list`  List all servers the irssi knows about, connected or not.

`/connect freenode` Connect to freenode without disconnecting from current
servers.

`/disconnect`   Disconnect from the current server.

`/disconnect freenode [message]`    Disconnect from freenode with an optional
bye message.

`/discconect * [message]`   Disconnect from all servers.


## Channels

To join a channel:

    /join #channel

To leave a channel:

    /part #channel [message]


### Automatically Join Channels when Irssi Starts

    /channel add -auto #ubuntu freenode
    /save


### Join multiple channels in the same window

This can be very useful if you use multiple, low-traffic channels and want to
easily keep an eye on and participate in them all at once:

    /join -window #channel

Join `#channel` but instead of creating a new window for it, mix messages from
the new channel into the current window with the current channel. You can mix
as many channels as you want into a single window.

You'll want to turn on `print_active_channel` so it prints the channel name
next to each message:

    /set print_active_channel on
    /save

Messages that you type will only be sent to one channel (the one shown in the
status bar), to change which channel you're sending to use `/join #channel` as
normal.

If you want to save your window layout (which channels are in which windows,
and which channels share the same window) do:

    /layout save
    /save

`/layout_save` saves the window locations of your channels so if you leave a
channel and later join it again, it appears in the same window.

The way to get irssi to open two channels in the same window automatically on
startup seems to be:

1. If you already have both channels open in different windows, leave one of
   them: `/part channel_two`
2. `/join #channel_one` if you haven't already, if you have then switch to its
   window
3. `/join -window #channel_two`
4. `/layout save`
5. `/save`
6. If you haven't already, tell irssi to auto join the channels on open:
   `/channel add #channel_one network`, `/channel add #channel_two network`.

Now if you `/quit` and restart irssi, the two channels should automatically
open in the same window.


## Notifications

Get notified when someones away status changes:

    /notify -away kindly freenode 

To remove a notification:

    /unnotify kindly

Print a list of the users in your notify list to the status window, saying
whether each is currently online or offline:

    /notify

Print a list of your configured notifications to the status window:

    /notify -list

TODO: Where do these notifications actually show up?

## Hilights

Lines that mention your nickname will get highlighted by default (the
`hilight_nick_matches` setting is ON by default). You can also configure custom
highlights:

    /hilight text

List your configured hilights in the status window:

    /hilight

Remove a hilight:

    /dehilight 1


## Beep on Mention

Irssi will highlight your nick when it's mentioned in a channel by default,
but that doesn't help much unless you're looking at the channel
window when it happens. To get irssi to beep when your nick is mentioned:

    /set bell_beeps ON
    /set beep_msg_level MSGS NOTICES DCC DCCMSGS HILIGHT

What "beep" actually means (play a sound, flash the terminal window) depends
on your terminal emulator's settings.

## Automatic Logging

    /set autolog on
    /set autolog_path = ~/.irssi/logs/$X/$tag/$0.log
    /save

All messages to a channel or to you privately (but not other types of
messages) will be logged to files


## Away Logging

Irssi can conveniently print any important messages that happened while you
were away to the status window when you come back.

To set yourself away do:

    /away <reason>

`<reason>` is mandatory. You can type the command again to change your away
reason while staying away.

To set yourself as available again do:

    /away

By default private messages to you and messages that match any of our hilights
are logged while you're away, and printed to the status window when you come
back. I think these are the default settings:

    /set awaylog_level = msgs hilight
    /set awaylog_file = ~/.irssi/away.log
    /save


## IRC Commands

`/names` See who's in the channel

`/whois`

`/clear`

`/msg` Send a private message to someone

`/topic` See the channel's topic


TODO:

-  Unity integraton for notifications, I'm currently using
   <http://code.google.com/p/irssi-libnotify/> but it doesn't do the Unity
   messaging menu
-  The status bar, what does it mean and how can you configure it?
