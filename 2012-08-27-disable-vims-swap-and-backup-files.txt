They might let you recover some lost content one day, but Vim's backup and swap
files and the `ATTENTION` swap file exists messages irritate me every day. I'll
take my chances with git and daily backups to an external hard drive. Put this
in your vimrc file to turn them off:

    set nobackup
    set nowritebackup
    set noswapfile
