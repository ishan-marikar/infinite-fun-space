Open mutt to write and send a mail to shannon (a name from my mutt
address book) right away, without opening any mailbox first:

    mutt -s "Hi there" shannon

You can send a message to multiple recipients by just giving a
space-separated list of addresses to the command:

    mutt -s "Hi guys" ryan alex someone@example.com

You can specify **c**c and **b**cc addresses with `-c` and `-b`.  
If you want to specify multiple cc or bcc options give multiple `-c`
or `-b` args, don't give space-separated lists here.

You can **i**nclude the contents of a file into the body of the
message with `-i`.

You can **a**ttach files to the message with `-a`:

    mutt -s "Picture of Buzz" -a buzz.jpeg -- alex

`-a` takes a space-separated list of files (and patterns that match
files, e.g. `*.png`). It has to be the last option in the command
and should be followed by `--` before the list of addresses.

See `man mutt`.
