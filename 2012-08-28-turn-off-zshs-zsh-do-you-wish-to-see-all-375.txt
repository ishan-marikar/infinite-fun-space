This message from zsh always gets in the way when I want to see tab-complete
suggestions for command options, zsh prints it whenever the list of
possibilities is longer than a screen's worth, .e.g. if I type:

    git log -<tab>
    rsync -<tab>
    etc.

To turn it off put this in your zshrc file:

    # Don't give the "zsh: do you wish to see all 375 possibilities (125
    # lines)?" messages unless it's more than 1000 lines.
    LISTMAX=1000

Now I can just type `rsync -<tab><tab>` and then use the tab key (and arrows)
to browse the list of rsync options and their short help texts unhindered.
