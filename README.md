# dlorg
##Task 0
### Oa
dlorg repo made > dlorg_Johan_V

### 0b
added .gitignore a finshed tempplate from github to ignore all vim files in the background.
used claude LLM on how to add the ignore for vim files, since i already know how to start
and make a repo.

### 0c
made a file called dlorg, empty file for linux with previliges changed from 664 to 744, so
people can read it but not execute or change it except for me.

### 0d
chmod 744 dlorg and understood now what 755 came from in chmod and now got a deeper understanding
how files work, and it made me wonder instead of -rwx etc why not as numbers for easier to read?
apparently you can change the way how you look at previliges to with a diffrent command where
it shows numbers. like how i changed mine to 744 from 664.

## Task 1
### comment1
after installing inotify-tools i checked with "dpkg -L inotify-tools" to check
what tools i have under my belt (inotify-tools = toolbelt) and learned that i have 2 tools
inotifywait and inotifywatch. got corrected by claude LLM that i actually have 4 tools
inotifywait, inotifywatch, fsnotifywait, fsnotifywatch. and i learned that its a diffrence.
fs-tools are more system wide while i-tools are more specific like this folder or file.

### comment2
`create` - a file or directory was created within a watched directory
`moved_to` - a file or directory was moved into a watched directory

from the looks of it i need these 2 events, double checking
if i need more events so i dont get bugs down the line.

`close_write` couldnt understand why `close_write`  was needed over "close" but was pointed out in ma>
that close_write watched file or file within a watched directory was closed, AFTER being
opened in writable mode. since even when we use the command "touch" or "cp" it makes an empty
file, but it still opens in writable mode, so the alarm goes off for the folder we watching.
had to make a double check with claude and pushed back, but got a good deeper explanation
for it.
