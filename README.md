# dlorg – Downloads organizer

dlorg keeps your `~/Downloads` folder tidy. Every time a file finishes downloading
or is moved into Downloads, dlorg moves it into a matching folder inside `~/Downloads/Sorted/`.
It runs in the background as a systemd user service, so you never have to start it yourself.

**Before:** example of a messy Downloads folder. (Staged in a separate test folder, because in the real Downloads dlorg sorts files the moment they arrive.)
![messy screenshot, placeholder](screenshots/Messy-pictures-2026-10-08.png)

**After:** dlorg sorted everything into Sorted
![sorted files](screenshots/Sorted-folder-2026-10-08.png)

## Where files go

| Folder | File types |
|---|---|
| Pictures | jpg, jpeg, png, gif, webp, svg, bmp |
| Videos | mp4, mkv, avi, mov, webm |
| Audio | mp3, flac, wav, ogg, m4a |
| Documents | pdf, doc, docx, odt, txt, md, rtf |
| Spreadsheets_Slides | xls, xlsx, ods, csv, ppt, pptx |
| Archives | zip, tar, gz, 7z, rar, xz |
| Installers | deb, rpm, exe, msi, appimage, iso |
| Code | sh, py, js, html, css, c, json, cpp, cs |
| Other | everything else, and files with no extension |

Folders are created automatically when needed, and recreated if deleted.

**Sorted:** the Sorted folder with its sub folders organized
![Sorted folder with its sub-folders named](screenshots/Sorted-sub-folders-2026-10-08.png)

## Requirements

- Linux with systemd (tested on Debian 13)
- `inotify-tools`: `sudo apt install inotify-tools`

## Installation

1. Clone the repo:
   `git clone https://github.com/johanVcode/dlorg_Johan_V.git`
2. Copy the script into your personal bin folder and make it read + run only:
```bash
   mkdir -p ~/.local/bin
   cp ~/dlorg_Johan_V/dlorg ~/.local/bin/dlorg
   chmod 544 ~/.local/bin/dlorg
```
3. Install the service file:
```bash
   mkdir -p ~/.config/systemd/user
   cp ~/dlorg_Johan_V/dlorg.service ~/.config/systemd/user/
```
4. Turn it on (starts now and at every login):
```bash
   systemctl --user daemon-reload
   systemctl --user enable --now dlorg
```
5. Optional: start dlorg at boot, even before you log in:
   `loginctl enable-linger`

## Using it

If a file with the same name already exists, the new one gets a number. cat.jpg → cat_1.jpg.
**Sorting:** download or move a file into `~/Downloads` and it gets sorted.
![cat.jpg and testing123.png sorted into "Pictures"](screenshots/Sorted-pictures-2026-10-08.png)

## Checking that it runs

- Status: `systemctl --user status dlorg` → should say **active (running)**

**STATUS:** information about the daemon running
![Status log in ssh powershell terminal](screenshots/Status-log-2026-10-09.png)

## Stopping / removing

- Stop for now: `systemctl --user stop dlorg`
- Stop starting automatically: `systemctl --user disable dlorg`

## Updating

After changing the script in the repo, copy it again:
```bash
chmod u+w ~/.local/bin/dlorg
cp ~/dlorg_Johan_V/dlorg ~/.local/bin/dlorg
chmod 544 ~/.local/bin/dlorg
systemctl --user restart dlorg
```

## Known limitations

- Uppercase extensions (`PHOTO.JPG`) go to Other.
- Files already in Downloads before dlorg starts are not sorted.
- Sorting is by extension only (a MIME fallback is planned).

## Dev notes
See [Notes.md](Notes.md) for how dlorg was built, my decisions, and what I learned.
