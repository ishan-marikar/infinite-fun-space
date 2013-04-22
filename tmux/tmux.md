tmux
====


Notes about [tmux][] from various sources, including:

* [tmux: Productive Mouse-Free Development][]
* [Practical Tmux][]
* [Arabesque's tmux posts][]
* [Tmux on the ArchWiki][]
* [Dr. Bunsen's _The Text Triumvirate_][]
* Hawk Host's tmux blog posts [one][hh1] and [two][hh2]
* [`man tmux`][]

[tmux]: http://tmux.sourceforge.net/ "tmux is a terminal multiplexer"
[tmux: Productive Mouse-Free Development]: http://pragprog.com/book/bhtmux/tmux
[Practical Tmux]: https://mutelight.org/practical-tmux
[Arabesque's tmux posts]: http://blog.sanctum.geek.nz/category/tmux/
[Tmux on the ArchWiki]: https://wiki.archlinux.org/index.php/Tmux
[Dr. Bunsen's _The Text Triumvirate_]: http://www.drbunsen.org/the-text-triumvirate/
[hh1]: http://blog.hawkhost.com/2010/06/28/tmux-the-terminal-multiplexer/
[hh2]: http://blog.hawkhost.com/2010/07/02/tmux-%E2%80%93-the-terminal-multiplexer-part-2/
[`man tmux`]: http://manpages.ubuntu.com/manpages/precise/man1/tmux.1.html


Commands
--------

All tmux commands can be entered in any of four ways:

1. By hitting a bound keyboard shortcut such as `ctrl-a c` to create a new
   window.

2. By hitting `Ctrl-a :` to enter tmux's command-line, then typing the command
   and hitting `Enter`.

3. As a subcommand of the `tmux` command in a shell, eg. `tmux split-window`.

   Running tmux commands from zsh is usually better than using tmux's
   command-line, in zsh there is tab-completion of tmux subcommands and options
   with docstrings.

4. As a line in your `.tmux.conf` configuration file.

The names of the commands and their options are the same, however the command
is entered.

You can see a list of all tmux commands by doing:

*  `:list-commands` on the tmux command-line
   (i.e. type `Ctrl-a :list-commands<Enter>` in tmux)

* `$ tmux list-commands` in a shell

*  or just `$ tmux <tab>` in zsh


Configuration File
------------------

The tmux system-wide and user config files are:

    /etc/tmux.conf
    ~/.tmux.conf

The `set-option` command (alias: `set`) sets config options. For example, to
change tmux's prefix from the default `Ctrl-b` to `Ctrl-a`, put this in your
`.tmux.conf` file:

    # Bind Ctrl-a instead of Ctrl-b as prefix key.
    unbind C-b
    set -g prefix C-a

then restart tmux. `-g` means "global" which means set this option for all new
tmux sessions.


### Install the configuration file from this folder

To install [the .tmux.conf file from this folder](tmux.conf), run this command
in a shell:

    $ wget 'https://raw.github.com/seanh/content-addressable/master/tmux/tmux.conf' -O ~/.tmux.conf


### Reload your tmux.conf while tmux is running

If you've changed your `.tmux.conf` file, you can reload it without leaving
your tmux session with this command:

    source-file ~/.tmux.conf

You can create a keyboard shortcut `Ctrl-a r` to reload your `.tmux.conf` file,
by putting this in your `.tmux.conf` file:

    # Bind `Ctrl-a r` to reload the tmux.conf file.
    bind r source-file ~/.tmux.conf \; display "~/.tmux.conf reloaded!"


### 256 colours

To make sure 256-colour mode works inside tmux, add this to your `.tmux.conf`:

    # Enable 256 colours in tmux.
    set -g default-terminal "screen-256color"

_and_ always run tmux with the command `TERM=screen-256color-bce tmux`. You can
alias `tmux` to this command in your zshrc or bashrc:

    alias tmux="TERM=screen-256color-bce tmux"


Keybindings
-----------

Tmux commands can be bound to keyboard shortcuts, such as `Ctrl-a c` to open a
new window. The `bind-key` and `unbind-key` commands change keyboard shortcuts.

The `list-keys` command (bound to `ctrl-a ?` by default) lists all current
keybindings.

`bindkey -n` binds a key without having to press the prefix key first, eg:

    # Use Ctrl-PgDn and Ctrl-PgUp to change windows.
    bind-key -n C-PgDn next-window
    bind-key -n C-PgUp previous-window

This completely disables these key combinations in any app inside tmux that
might want to use them, so take care and use few of these.

`-r` makes a keybinding repeatable, so you can enter it multiple times without
having to hit `Ctrl-a` each time.


Sessions
--------

Create a new session named foobar:

    new-session -s foobar
    new -s foobar

Create a new session named foo, and name the session's first window bar:

    new -s foo -n bar

List all currently running sessions:

    list-sessions
    ls

Attach to an already-running session named foobar:

    attach-session -t foobar
    attach -t foobar

`attach` works both from within a tmux session (to change to a different
session) or from outside of any tmux session (to join one) (in this case
`attach` has to be run from the shell, e.g. `$ tmux attach -t foobar`).

In zsh, `$ tmux attach -t <tab>` will autocomplete the names of currently
running sessions to attach to.

Kill a session named foobar:

    kill-session -t foobar


Windows
-------

"Tabs" in tmux are called windows. The `new-window` command creates a new
window and changes to it (this is bound to `Ctrl-a c` by default).

By default a shell will be run in the new window. To run some other program
instead, give it as argument:

    new-window htop

So you can make keybindings to, for example, move to a joined window named
"mutt" or create a new window running mutt if there isn't one already.

`Ctrl-a ,` renames the current window. From the command line you can rename any
window:

    rename-window -t 1 foo

(without the `-t` option it would rename the current window).

There are various commands and keyboard shortcuts for moving between windows:

    ctrl-a ctrl-a
    ctrl-a n
    ctrl-a p
    ctrl-a 0...9
    ctrl-a f    List windows matching a search
    ctrl-a w    List all windows
    select-window -t 0..9

Kill a window:

    ctrl-a &


Panes
-----

Add this to your config to get easier-to-remember key bindings for splitting
windows:

    # Bind `Ctrl-a |` and `Ctrl-a -` to split the current window vertically and
    # horizontally.
    bind | split-window -h
    bind - split-window -v

To move between panes, use:

    Ctrl-a {left,right,up,down}

Cycle through built-in pane layouts:

    ctrl-a space

Select a specific layout directly:

    select-layout main-horizontal

(if you do this from zsh, you can tab-complete the layout names).

Kill a pane:

    ctrl-a x

(also closes the pane's window, if the window doesn't have any other panes).

"Pop out" a pain into its own window:

    ctrl-a !

Move the current pane "up" or "down":

    ctrl-a }
    ctrl-a {

(panes are numbered in the order they're created, these commands move the
current pane up and down in this sequence by swapping it with "adjacent"
panes).

Turn a window into a pane within another window:

    join-pane -s panes:1

"panes" is the name of the session you want to take a window from, 1 is the
number of the window you want to take. The window will become a pane in the
current window.

To join a specific pane instead of an entire window, do:

    join-pane -s panes:1.2

to take pane 2 of window 1 of session "panes".


### Resizing Panes

Add this to your config:

    # Bind `Ctrl-a shift-{left,right,up,down}` to resize the current pane.
    # These are repeatable (-r), for example you can do `Ctrl-a shift-left` and
    # then, without releasing shift, keep pressing left, up, down and right to
    # resize the pane as much as you want.
    bind -r S-Left resize-pane -L 1
    bind -r S-Right resize-pane -R 1
    bind -r S-Up resize-pane -U 1
    bind -r S-Down resize-pane -D 1


### "Maximising" and Unmazimising Panes

Note: tmux 1.8 (not yet available in Ubuntu 12.04, unless you install from
source) has a built-in "zoom pane" feature that replaces this.

Bind `ctrl-a M` and `ctrl-m` to temporarily maximise and then unmaximise the
current pane:

    bind M new-window -d -n tmp \; swap-pane -s tmp.1 \; select-window -t tmp
    bind m last-window \; swap-pane -s tmp.1 \; kill-window -t tmp

These bindings work by creating a new window called tmp, then swapping the
current pane with the new pane that tmux automatically created in the new
window. Unmaximising swaps the panes back then kills the temporary window.

(Unfortunately I think these keybindings misbehave if you change windows or
panes while the pane is maximised, or if you try to unmaximise a pane that is
not maximised.)


Managing Multple Sessions
-------------------------

Show a list of all running sessions so you can choose one to go to:

    ctrl-a s

Move window 9 of the ckan session to window 3 of the foobar session:

    tmux move-window -s ckan:9 -t foobar:3


### Create or attach to a named session

This bit of shell script will attach to a session named foobar if one exists,
or create a new session named foobar otherwise:

    $ tmux attach -t foobar || tmux new -s foobar


### Automatically connect to a named tmux session when starting your shell

Sometimes you open a new terminal emulator window, start working in a shell or
terminal app, then realise you want to open another tmux window or pane within
the terminal emulator window. To do so you have to quit the app you're running
and start tmux, which is annoying.

You can put this in your zshrc or bashrc file to automatically connect to a
named tmux session whenever you open a new shell:

    # Automatically connect to tmux when starting zsh.
    # If tmux is installed, and we're not already in a tmux session, then
    # attach to a named tmux session (creating it if it doesn't already exist)
    # when starting zsh.
    if tmux -V >/dev/null 2>&1; then
      if [[ "$TERM" != "screen-256color" ]]
      then
        tmux attach-session -t "$USER" || tmux new-session -s "$USER"
        exit
      fi
    fi

This is perfectly safe, the code won't execute if tmux isn't installed, you'll
just get a normal non-tmux shell.

Warning, this is a little weird:

- Remember to use `ctrl-a d` not `ctrl-d` to close a terminal window, `ctrl-d`
  will close all the terminal windows attached to the session!

- If you have multiple terminal windows open and attached to the same tmux
  session, tmux will force each terminal to be as small as the smallest one

- If you want to get out of the default session and attach to some other tmux
  session, just use the `attach -t <session_name>` command or `ctrl-a s`.  Or
  to start a new session and join it, use the `new` command. You can also use
  tmuxinator (below) from a shell to move from one session into another, e.g.
  `mux my_project`.


### Manage projects with tmuxinator

[Tmuxinator](https://github.com/aziz/tmuxinator) lets you create named
"projects", where each project defines (in a yaml file) a tmux session with a
set of programs to run in a layout of windows and panes. You then use the `mux`
command (an alias for `tmuxinator`) to open a project, and tmuxinator starts
the tmux session, creates the windows and panes and runs the programs specified
in the yaml file.

You can also specify pre commands that should be run before starting the
session, a project root dir (that new shells in the session will start in) and
a socket name to use for the session.

To create the yaml file for a project (or edit it, if the project already
exists):

    $ mux open ckan

To attach to a project's tmux session (starting the session if it isn't already
running):

    $ mux ckan

(like tmux's `attach -t` command, this also works for switching between
sessions if you're already in another tmux session).

To list all defined projects:

    $ mux list


Copy Mode
---------

Config:

    # Enable vi-mode when in copy mode.
    set-window-option -g mode-keys vi

Enter copy mode:

    ctrl-a [

Leave copy mode:

    Enter

Moving around in copy mode (with vi-mode active):

    Arrows or h, j, k, l           Move slowly
    w and b                        Move by word
    f followed by any character    Jump to the next occurence of that character
                                   (on the same line)
    F                              Jump backwards
    Page Up, Page Down             Move a page at a time
    gg                             Jump to top of buffer
    G                              Jump to bottom of buffer
    ?                              Search upwards
    /                              Search downwards
    n                              Jump to next search match
    N                              Jump to previous search match
    $
    ^
    etc (see :list-keys -t vi-copy)


### Copying and Pasting

Add this to your config to enable vim-like key bindings for copying (yanking)
and pasting text in copy mode:

    # Select and copy text with v and y (instead of Space and Enter) like in vim.
    bind -t vi-copy v begin-selection
    bind -t vi-copy y copy-selection

    # Bind Escape to exit copy mode.
    bind -t vi-copy Escape cancel

    # Paste text with `ctrl-a p`.
    unbind p
    bind p paste-buffer

To copy text: enter copy mode (`Ctrl-a [`), move the cursor to where you want
your text selection to start from, press `v`, move the cursor to where you want
your selection to end, press `y`, tmux copies the text and leaves copy mode.

To copy an entire pane's text:

    capture-pane

To show the current contents of the paste buffer:

    show-buffer

Save the paste buffer to a file:

    save-buffer buffer.txt

From a shell, you can capture the contents of the current page and save them
to a file in one command:

    $ tmux capture-pane && tmux save-buffer buffer.txt

Tmux actually copies text into a new paste buffer each time you copy, and saves
all the old paste buffers in a paste buffer stack. With the `choose-buffer`
command tmux will show you the list of paste buffers and their contents, and
let you choose one to paste from.

    ctrl-a =
    choose-buffer

The paste buffers are shared across all running tmux sessions, so you can copy
text from one session and paste it into another.


### Using the X Windows Clipboard

Note: this doesn't work on OS X.

First, install `xclip`:

    sudo apt-get install xclip

Copying text in tmux's copy mode copies it into tmux's own paste buffer, not
into the system clipboard. Pasting text in tmux pastes from tmux's paste
buffer. If your terminal emulator supports it, you can still copy-paste with
the system clipboard using terminal emulator shortcuts. (In my terminal
emulator, select text with the mouse, right-click, and choose copy. To paste
from the system clipboard, `Shift-Ctrl-v`.) But this doesn't recognise the
boundaries between tmux panes, it will copy parts of text from adjacent tmux
panes at once, including the border characters between the panes. So these key
bindings can sometimes be useful to move text between tmux's paste buffer and
the system clipboard:

    # Copy text from tmux's paste buffer into the X clipboard with `ctrl-a y`.
    bind y run "tmux save-buffer - | xclip -i -sel clipboard" \; display "Text copied to X clipboard"

    # Paste text from the system clipboard into tmux with `ctrl-a P`.
    bind P run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"

Note: as your terminal emulator's text copying isn't aware of tmux's panes,
borders and status line and will copy them as if they're part of the content,
tmux's copy mode isn't aware of vim's gutter, status line, split windows, etc.
So if you want to copy text from vim into the system clipboard, it's best to do
this using vim keybindings.


Pair Programming
----------------

Unlike screen sharing, using tmux for pair-programming works even with slow or
unreliable network connections. It uses much less bandwidth.


### With a shared user account

The simplest way is to have a shared user account on a shared remote machine,
both ssh into the same user account and simply attach to the same tmux session:

- Create user account `tmux` on the shared remote server
- Add a `~/ssh/authorized_keys` file to the `tmux` account, with the right
  permissions
- Add both user's ssh public keys to the file
- First user ssh's into the `tmux` account on the remote machine and does
  `tmux new-session -s foobar`
- Second user ssh's into the `tmux` account on the remote machine and does
  `tmux attach -t foobar`


### Grouped Sessions

When you're both attached to the same session, you both see the same thing at
the same time. For example, if one user changes to a new window, all users
will change. If you don't want this, then instead of attaching to the other
user's session you can create a _new_ session and attach that session to their
session:

    tmux new-session -t foo -s bar

Now users can switch windows independently, but if one user creates a new
window all users will see it in their status bar.


### Tmux Sockets

With sockets different users (with different unix accounts) can connect to the
same tmux session. Instead of launching tmux with the `new-session` and
`attach` commands, both users launch tmux specifying the socket file to user
for the session with `-S`:

    tmux -S /var/tmux/pairing           (Creates the session)
    tmux -S /var/tmux/pairing attach    (Attaches to the session)

The socket file needs to be one that both users have permission to read and
write. The easiest way is to make a unix group called `tmux`, set the group of
`/var/tmux` to `tmux`, set `g+ws` permissions on `/var/tmux`, and add all the
users to the `tmux` group.

Unfortunately all sessions sharing the socket will use the `.tmux.conf` of the
user who started the first session.
