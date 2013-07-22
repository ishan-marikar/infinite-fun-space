---
title: How to use mutt's message cache
layout: post
type: text
---

Mutt can cache the headers and bodies of email messages, which makes searching
mail folders much faster (especially searching the full bodies of an IMAP
folder). Put something like this in your muttrc file:

    set header_cache=~/.mutt/headercache
    set message_cachedir=~/.mutt/messagecache

The catch is that mutt doesn't cache a message until it has downloaded that
message once. So a search like `~b foobar` in a large IMAP folder will take a
long time because mutt downloads the full body (and any attachments) of every
message in the folder. What's more, mutt's UI completely freezes while mutt
searches. But once it's finished a search like this once mutt has the body of
every message in that folder cached, so full body searches on that folder will
be much faster from then on.

The trick is to go into any large folders that you have: Sent, Archive, etc.
and do a search for `~b foobar` to seed mutt's cache so that you can search the
folders quickly when you actually need to find something. Ofcourse, if new
messages get added to these folders then you might need to repeat the process
now and then.

(Note that for simple search you can also use e.g. `=b foobar` instead of `~b
foobar` to use IMAP server-side search, which is much faster.)

<http://www.mutt.org/doc/devel/manual.html#patterns>  
<http://www.mutt.org/doc/devel/manual.html#tuning-search>  
<http://www.mutt.org/doc/devel/manual.html#caching>  
