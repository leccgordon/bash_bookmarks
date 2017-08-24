# The Bookmarks Tutorial
## Introduction
Bookmarks is a bash utility to remember directory and file locations.

Features include

* Add comments for each bookmark
* Groups and sub-groups for categorizing bookmarks
* Multiple linked bookmark files
* Derived names - bookmark a log file containing today's date
* Shareable with other users
* Fully searchable; search all linked bookmarks files

## Install and Set-up
### Prerequisites
bash v3 or greater

### Install
Copy file bm.env to appropriate location.

For the purposes of this tutorial, `bm.env` is located in `/home/eduardo`

For a more permanent location, use `/usr/local/share`

### Initialize
At the bash prompt, type `. /home/eduardo/bm.env init`

    eduardo@doxology:~ (0)
    $ . /home/eduardo/bm.env init
    Bookmarks v0.1 initialized
    Type 'bm help' for help
    eduardo@doxology:~ (0)
    $

This creates the bash shell functions bm and bma; these functions are used to manage your bookmarks.

To automatically enable bookmarks each time you start bash, add `. /home/eduardo/bm.env init` to your `.bashrc` file.

### Environment

Item | Description
--| --
`$BMLOC` |  Tracks your current bookmark file and group. If not set, ~/.bookmark is used.
`/home/eduardo/bm.env` |  The bookmarks code
`~/.bookmark` | The default bookmarks file. Override using $BMLOC
`~/.bookmark.cfg` | Optional config file. Overrides default bookmark settings
`bm` | bash function; interfaces to bm.env
`bma` | bash function; add items and groups 

### Uninstall
If bookmarks do not meet your needs, here are the steps to remove bookmarks from your system

1. Remove file bm.env
1. Remove any reference to bm.env from .bash_profile, .profile, .bashrc etc
1. Remove ~/.bookmark
1. Remove ~/.bookmark.cfg

## Bookmark a Directory
Change to a directory you want to remember, and type `bma`:

    eduardo@doxology:~ (0)
    $ cd /etc/init.d
    eduardo@doxology:/etc/init.d (0)
    $ bma
    
Alternatively, you can name the directory (using absolute or relative path):

    eduardo@doxology:/etc/init.d (0)
    $ bma /usr/local/share

To view your bookmarks, type `bm`:

    eduardo@doxology:/etc/init.d (0)
    $ bm
    -----------------------
    /home/eduardo/.bookmark
    -----------------------
      1. /etc/init.d/
      2. /usr/local/share/

    ['help' for more info]
    Item:2
    eduardo@doxology:/usr/local/share (0)
    $ 

The header shows the location of the current bookmark file.

A trailing slash differentiates directories from files.

When item 2 is picked, you are returned to the directory `/usr/local/share`

You can also pick the item as a parameter:

eduardo@doxology:/usr/local/share (0)
$ bm 1
eduardo@doxology:/etc/init.d (0)
$ 

## Bookmark a File
