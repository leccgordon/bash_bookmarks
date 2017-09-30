# Bookmarks Tutorial
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
Copy file `bm.env` to appropriate location.

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

This creates the bash shell functions `bm` and `bma`; these functions are used to manage your bookmarks.

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
In `/etc/init.d` we can bookmark `/etc/rc.local` using a relative path

    eduardo@doxology:/etc/init.d (0)
    $ bm ../rc.local
    
To bookmark a file in the current directory, simply name the file

    eduardo@doxology:/etc/init.d (0)
    $ bma cron
    
You can also use absolute path to bookmark a file

    eduardo@doxology:/etc/init.d (0)
    $ bma /etc/hosts

View the bookmarks file again:

    eduardo@doxology:/etc/init.d (0)
    $ bm
    -----------------------
    /home/eduardo/.bookmark
    -----------------------
      1. /etc/init.d/
      2. /usr/local/share/
      3. /etc/rc.local
      4. /etc/init.d/cron
      5. /etc/hosts

    ['help' for more info]
    Item:

When item 5 is picked, the file /etc/hosts is opened using the default editor for bookmarks, which is vi.
To change this to another editor, see bookmark.cfg settings

## Create a bookmark group
Once many items are bookmarked, the bookmarks file can get unwieldy.
When this happens, bookmarks can be categorised into groups.

To add a group called `bootup` use `bma` as follows:

    eduardo@doxology:/etc/init.d (0)
    $ bma group bootup
    Group bootup added
    

Now, when you type `bm` you get the following:

    eduardo@doxology:/etc/init.d (0)
    $ bm
    --------------------------------
    /home/eduardo/.bookmark (bootup)
    --------------------------------
     g1.  group= 
     g2. (group=bootup)
     
     ['help' for more info]
     Item:
     
To navigate, use the items prefixed with "g". The group list shows you
1. each group from the root group to the current group
1. the current group
1. each available group one level below this

You can also add a group within a group:

    eduardo@doxology:/etc/init.d (0)
    $ bma group network
    Group bootup:network added
    
    eduardo@doxology:/etc/init.d (0)
    $ bm
    ----------------------------------------
    /home/eduardo/.bookmark (bootup:network)
    ----------------------------------------
     g1.  group= 
     g2.  group=bootup
     g3. (group=bootup:network)
    
    ['help' for more info]
    Item:


When you return to the prompt, your group location is preserved in the environment variable `$BMLOC`

    eduardo@doxology:/etc/init.d (0)
    $ echo $BMLOC
    bootup!network@/home/eduardo/.bookmark
    
## Edit bookmarks file
To edit the bookmarks file, type `edit`

    ----------------------------------------
    /home/eduardo/.bookmark (bootup:network)
    ----------------------------------------
     g1.  group= 
     g2.  group=bootup
     g3. (group=bootup:network)
     
    ['help' for more info]
    Item:edit
    
The current bookmarks file is opened using the default bookmarks editor, which is `vi`.
To change the bookmarks editor, see bookmark.cfg settings

## Display formatting
There are two commands you can use in the bookmarks file to document the bookmarks.

Command | Description
----|----
print |  Print a line of text
head  |  Print a blank line followed by a line of text 

Example bookmarks file with print and head commands added

    /etc/init.d
    print The location of bm.env
    /usr/local/share
    /etc/rc.local
    head cron service script
    /etc/init.d/cron
    /etc/hosts
    
    
    rem ----------
    ==group bootup
    rem ----------
    ==end-group bootup
    
    
    rem ------------------
    ==group bootup:network
    rem ------------------
    ==end-group bootup:network
    
When viewed

    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local
    
    cron service script
      4. /etc/init.d/cron
      5. /etc/hosts
    
    ['help' for more info]
    Item:
    
## Run command
You can execute a command using bookmarks.
This feature is disabled by default, but can be enabled by typing `config` and setting the flag for `enable_run` to `Y`.

    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local
    
    cron service script
      4. /etc/init.d/cron
      5. /etc/hosts
    
    ['help' for more info]
    Item:config
    
Change `enable_run` to `Y`

    enable_clear=Y
    editor=vi
    enable_run=Y
    
Next, add an item to run. The following will be added as an example

    run ls -lrt /tmp
    
Edit the bookmarks file and add the line above after `/etc/hosts`

    eduardo@doxology:/etc/init.d (0)
    $ bm edit

    /etc/init.d
    print The location of bm.env
    /usr/local/share
    /etc/rc.local
    head cron service script
    /etc/init.d/cron
    /etc/hosts
    run ls -lrt /tmp
    
    
    rem ----------
    ==group bootup
    rem ----------
    ==end-group bootup
    
    
    rem ------------------
    ==group bootup:network
    rem ------------------
    ==end-group bootup:network
    

Now pick the new item

    eduardo@doxology:/etc/init.d (0)
    $ bm
    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local
    
    cron service script
      4. /etc/init.d/cron
      5. /etc/hosts
      6. run ls -lrt /tmp
    
    ['help' for more info]
    Item:6
    ------------------------------
    ls -lrt /tmp
    total 24
    drwx------ 2 eduardo eduardo 4096 Jan  1  1970 orbit-eduardo
    drwx------ 2 lucia   lucia   4096 Jan  1  1970 orbit-lucia
    drwx------ 2 root    root    4096 Apr  1 08:32 pulse-PKdhtXMmr18n
    drwx------ 2 eduardo eduardo 4096 Apr  1 20:03 ssh-lomVYaQRBRne
    drwx------ 2 eduardo eduardo 4096 Apr  1 20:03 keyring-nD3Np9
    drwxrwxrwx 2 lucia   lucia   4096 Apr  1 20:04 mintUpdate
    ------------------------------
    $?=0
    Press Return to continue
    
## Other item commands
### Navigate to item's directory
You can navigate to the directory containing an item using the `dir` command

    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local
    
    cron service script
      4. /etc/init.d/cron
      5. /etc/hosts
      6. run ls -lrt /tmp
    
    ['help' for more info]
    Item:dir 5
    eduardo@doxology:/etc (0)
    $ 
    
### Select multiple items
Multiple file entries can be selected to be viewed sequentially

    -------------------------------
    /home/eduardo/.example_bookmark
    -------------------------------
      1. /home/eduardo/file_A.txt
      2. /home/eduardo/file_B.txt
      3. /home/eduardo/file_C.txt
      4. /home/eduardo/file_D.txt
      5. /home/eduardo/file_E.txt
    ['help' for more info]
    Item:
    
To select all five items, the following are equivalent

    Item:1 2 3 4 5
    Item:1-5
    Item:1-3 4 5
    
Note that if you include a directory or run command in a multiple selection, it is ignored.

### Return to top-level
You can return to the top-level group of the top-level bookmark file using the `root` command

    ----------------------------------------
    /home/eduardo/.bookmark (bootup:network)
    ----------------------------------------
     g1.  group= 
     g2.  group=bootup 
     g3. (group=bootup:network)
    
    ['help' for more info]
    Item:root
    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local

    cron service script
    4. /etc/init.d/cron
    5. /etc/hosts
    6. run ls -lrt /tmp

    ['help' for more info]
    Item:

## Referencing another bookmark file
Use command bmloc to add a link to another bookmark file.

    -------------------------------
    /home/eduardo/.example_bookmark
    -------------------------------
      1. /home/eduardo/file_A.txt
      2. /home/eduardo/file_B.txt
      3. /home/eduardo/file_C.txt
      4. /home/eduardo/file_D.txt
      5. /home/eduardo/file_E.txt
      6. bmloc /home/eduardo/.another_bookmark_file
    ['help' for more info]
    Item:6
    ------------------------------------
    /home/eduardo/.another_bookmark_file
    ------------------------------------
     g1. (group=)
     g2.  group=var
    
      1. /home/eduardo/debug.out
    
      2. Return to previous bookmark file /home/eduardo/.example_bookmark
    
    ['help' for more info]
    Item:

If you return to the prompt and restart bookmarks, you will retain your current bookmark state

    ------------------------------------
    /home/eduardo/.another_bookmark_file
    ------------------------------------
     g1. (group=)
     g2.  group=var

      1. /home/eduardo/debug.out

      2. Return to previous bookmark file /home/eduardo/.example_bookmark

    ['help' for more info]
    Item:
    
## Make group hidden
If you have groups you no longer want displayed but do not wish to delete, the convention is to prefix the group name with `hidden:`

The contents of the group will still be searchable.

An alternative is to copy the contents to another bookmarks file

## Finding items
You can find items using by prefixing search string with /.

This can be done from command line or in bookmark menu.

All items and group names are searched, and all bookmark files linked with `bmloc` are also searched.

    eduardo@doxology:~ (0)
    $ bm /hello
    -------------------------------
    /home/eduardo/.example_bookmark
    -------------------------------
     g1.  group=/home/eduardo/.demo_bookmark#var:www
    
      1. [/home/eduardo/.demo_bookmark#var:www] /var/www/cgi-bin/hello2.pl
      2. [/home/eduardo/.demo_bookmark#var:www] /var/www/cgi-bin/hello.pl
    
    ['help' for more info]
    Item:
    
The results above show us there are two matches in one group within the bookmark file `/home/eduardo/.demo_bookmark`
If you choose item `g1`, the bookmark file `/home/eduardo/.demo_bookmark` is opened in group `var:www`
If you choose item 1 or 2, the file is opened using the defined editor.

## Redirect output
When output is redirected, picked items are instead sent to stdout. Note that both directories and files items are output during redirection.

    $ bm 1-5 |cat
    /etc/init.d/
    /usr/local/share/
    /etc/rc.local
    /etc/init.d/cron
    /etc/hosts
    eduardo@doxology:/etc (0)
    $ 
    
Any spaces in the pathname are replaced with `[[.space.]]` when redirected. 

    $ bm 12 |cat
    /home/eduardo/some[[.space.]]file.txt
    
This allows effective command substitution for filenames containing spaces and other unusual characters

    $ md5sum $(bm 12)
    2d01d5d9c24034d54fe4fba0ede5182d  /home/eduardo/some file.txt
    
If you do not want the space substitution to occur, prefix the item or items with `raw`

    $ bm raw 12 |cat
    /home/eduardo/some file.txt
    
## Help
Use `bm help` to list the standard bookmark options 

    eduardo@doxology:~ (0)
    $ bm help
    
      Environment:
        Bash Version 4.1.2(1)-release
        BMLOC="/home/eduardo/.bookmark"
        |BMLOC defaults to ~/.bookmark if not set
    
      Command line:
        bm                  : show current group bookmarks and pick item
        bma                 : add current directory to bookmarks
        bma file            : add full path of file to bookmarks
        bma dir             : add full path of dir to bookmarks
        bma *.txt           : add files matching *.txt to bookmarks
        bma group eg1       : create group "eg1" (within current group)
        |group name cannot contain the following 3 characters ":@!"
    
      Item:
        1			: select item 1; edit, cd or set group accordingly
        1-10		: edit all files within items 1-10
        dir 2		: cd to directory containing item 2
        raw 3               : when stdout redirected, list item 3 without replacing spaces with [[.space.]]
        root		: set to root group and display
        /str		: list items matching "str" in all group sections
        /group		: list all group sections
        edit		: edit bookmarks file
        config              : edit config (~/.bookmark.cfg)
        |item can be parameter eg "bm 1-5", "bm raw 3", "bm /myfile", "bm edit"
        |if stdout redirected, filenames are output eg "bm >files.txt"
    
      Bookmark file:
        rem                 : ignore line
        print text          : print text
        head text           : print section heading
        run command         : execute command when selected
        bmloc filename      : open bookmark file filename when selected
        ==group eg1         : start of group "eg1" block
        ==end-group eg1     : end of group "eg1" block
        |directives must start on first character of line
        |bookmark may be specified as wildcard eg "/full/path/*.txt"
        
## bookmark.cfg settings
To edit config, use `config` command
    
    -----------------------
    /home/eduardo/.bookmark
    -----------------------
     g1. (group=)
     g2.  group=bootup 
    
      1. /etc/init.d/
    The location of bm.env
      2. /usr/local/share/
      3. /etc/rc.local
    
    cron service script
      4. /etc/init.d/cron
      5. /etc/hosts
    
    ['help' for more info]
    Item:config

    enable_clear=Y
    editor=vi
    enable_run=Y
    
### standard attributes
#### enable_clear
turn on and off clear screen within bookmarks
#### editor
editor to use. Defaults to vi
#### enable_run
enable/disable run command

### optional attribute

#### basedir.IDENTITY
path prefix substitution for bookmarks. See "Referencing bookmarks on shared drive from different machines" for details

## Bookmark variables
You can define variables and use these in bookmark files.

A defined variable is referenced using `%{var}`, where var is the variable.

You cannot reference environment variables directly using `$var`.

In the bookmarks file, the following advanced commands are available
### set var=value
Assign value to `var`

    set currenv=/data/unit_test
    
### read var from env
Read variable `var` from current environment

    read LOGNAME from env
    read mylogname from env LOGNAME
    
### read var from file /full/path/to/file
Read value from first line of file

    read batch_date from %{currenv}/today.dat
    
### read var from file /full/path/to/file using prefix_token
Read value from file where line matches `prefix_token`

    read batch_date from %{currenv}/runvars.cfg using today=
    
### read var from date +date_format
Read current date into var using date format 

    read today_var from date +%Y%m%d
    
### read var from date +date_format specified_date
Read current date into var using date format for specified date

This will only work on systems that support `date --date` option

    read yesterday_var from date +%Y%m%d yesterday
    
### read var from [first|last] glob /full/path/to/file
Expands glob pattern and reads first/last item

    read last_run from last glob %{currenv}/logs/??????/upload-????-??-??.log
    
### show vars
Dump all defined bookmark variables. This will include config and internal variables. 

    show vars
    
### global vs local scope
If a variable is defined at root-level, it is global, and visible in all groups.

If a variable is defined within a group, it overrides a global variable with the same name.

Each group's variables are private - there is no inheritance from a group to a sub-group.

### Referencing config variables
Config variable are prefixed with config. but are referenced in the same way as other variables.

    print The current editor is %{config.editor}
    
### Read value as current bookmark line
If the special variable . is assigned, the value is immediately output.

    read . from last glob %{currenv}/logs/??????/upload-????-??-??.log
    
is equivalent to

    read last_run from last glob %{currenv}/logs/??????/upload-????-??-??.log
    %{last_run}
    
## Referencing bookmarks on shared drive from different machines
If a bookmark file is stored on a shared drive and referenced by two machines with different base paths for the mount point, you can set a config item to manage this.
The following example illustrates how this feature may be used. The machine names are `doxology` and `talisker`


First, set the variable `current.identity` in the bookmark file to identify it

`doxology:/home/eduardo/nas02_eduardo/.bookmark`

    set current.identity=nas02
    
On `doxology`, add the following entry to the config file

`doxology:/home/eduardo/.bookmark.cfg`

    basedir.nas02=/home/eduardo/nas02_eduardo
    
On `talisker`, add the following entry to the config file

`talisker:/usr/eduardo/.bookmark.cfg`

    basedir.nas02=/usr/eduardo/nas02
    
When a bookmark that matches `basedir.nas02 prefix` is added, the basedir prefix is replaced with `%{config.basedir.nas02}`

This ensures the path resolves correctly on both machines.

For example, if on `doxology` we add the following file

    eduardo@doxology:~ (0)
    $ bma /home/eduardo/nas02_eduardo/todo.txt
    
This will be stored in the bookmark file as

    %{config.basedir.nas02}/todo.txt
    
If we then access this bookmark from `talisker`, this bookmark will be resolved to

    /usr/eduardo/nas02/todo.txt
    
## Shareable bookmarks
If users require to share bookmarks, it is recommended to set up a separate location for this rather than sharing bookmarks from the user's home directory.

A commonly used option is to create the directory `/etc/bookmark` (owned by root), and create a user-owned directory for each user within this.

Each user can now create and share bookmarks safely.

The following example shows the shared bookmark directory for users `eduardo` and `lucia`

    eduardo@doxology:~ (0)
    $ find /etc/bookmark -printf "%u:%g %p\n"
    root:root /etc/bookmark
    eduardo:eduardo /etc/bookmark/eduardo
    eduardo:eduardo /etc/bookmark/eduardo/shared_bookmark
    lucia:lucia /etc/bookmark/lucia
    lucia:lucia /etc/bookmark/lucia/shared_bookmark
