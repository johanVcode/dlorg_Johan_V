# dlorg

## Task 0
### 0a
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
`close_write` - a file that was opened for writing was closed (the file is finished)
`moved_to` - a file or directory was moved into a watched directory

I first planned to use `create`, but switched to `close_write`, `create` fires the moment a
file appears, even while a download is still being written, so dlorg could move a half-finished
file. `close_write` waits until the writing is done. I couldn't understand at first why
`close_write` was needed over plain `close`, but the man pages pointed out that `close_write`
only fires when the file was opened in writable mode. Even `touch` and `cp` open the file in
writable mode, so the alarm still goes off for the folder we are watching. I double-checked
with Claude and pushed back, and got a deeper explanation.

### comment3
learned that `set -u` is a great seatbelt for variables that are not present. 
Decisions: linked the paths (dry) +  added `set -u` why? if Downloads ever moves
i can just change 1 lineand Sorted follows. seperate paths risk a silent bug
where files get sorted into the old folder instead. then ill be debugging and trying
to find this bug, wasting time. `set -u` stops the script loudly if a variable name is mistyped
so my mistakes become loud instead of silent.

### comment4
today i learned that variables need to be set so the whole script can work, logically what i need to do

- check file type and decide basket (or other)
- make sure the basket exists, if not make the basket on demand
- same name check, if same name add a numeric value to sort same-name files
- move the files

first i was kinda lost, i wondered what is a variable? then i realised very quick that it's a box, and boxes can hold the contents of other boxes too, boxception.

i first called them "aliases", aka labels to use them with a simpler name than to type them out constantly,
which saves time and meets the DRY conditions. the real term is variable, which is an alias in bash is a nickname for a command.

also realised some tips and tricks with `#*.` & `%.*`: simply put, `#*.` removes the name and keeps what's after the dot (like `pdf`),
and `%.*` does the other way around, removes the extension and keeps the name. but i realised a bug: what happens if names are more complex? like a naming convention of "name.07-07.20XX.jpeg" so what do we do? we use a double hash `##*.`, which keeps only what's after the last dot, so weird naming conventions still give the right file extension.

Used Claude as a tutor in hints-only mode. Lines given on request: the base_name split, the while condition, the mv line, the inotifywait / while read loop, and the systemd unit file. The rest i worked out from hints, man pages, the Arch Wiki and testing myself using google.

### comment5

today i added `mv` and a `while read` loop, which catches each `new_file` name and hands it to `file_sorting`. i also have
a "VIP list" `case` in this matter for all the common file types, and i learned that we can add MIME detection on top of this to improve it even further.

- we made a doorbell with `inotifywait`
- we added a `while read` "loop" that catches every new file name that hits ~/Downloads
- our bouncer, aka `file_sorting` was added and we added a VIP list with `case`
- we also made the script into a daemon using a systemd user unit. I was split between `Restart=always` and `Restart=on-failure`.

it took me a while to understand what `always` and `on-failure` because `always` brings it back either way, while `on-failure` when it fails it does reboot but doesnt on a clean exit, we added "-m" to inotifywait
so im not sure here why it makes it better. I also learned that we can restrict it even further with `NoNewPrivileges=yes`, so it can never escalate to root.

### comment6 - jargon vs understanding
right now terminology is my weakness. it takes me repetition before technical names stick, so i learn by analogy first and attach the real term after. the whole script is a nightclub in my head: `inotifywait` is the doorbell, `file_sorting` is the bouncer, `case` is the VIP list, the extension is the guest's badge, and systemd is the building manager. i try to pair every analogy with its real name but it takes time for me
to have it stick.
