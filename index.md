**Again:** this repo contains all the files from my setup. This is a way of sharing something, but also for my own backup of knowledge about how I finished setting up some things that I had to deal before.

## Better font rendering   
- `sudo apt install ttf-mscorefonts-installer`
- `sudo apt install freetype2-demos`
- `sudo apt install libfreetype6`
- Download .deb of [fontconfig-infanility](https://software.opensuse.org/package/fontconfig-infinality)  
- Fix emojis with download [latest TwemojiColor](https://github.com/eosrei/twemoji-color-font/releases) Linux release, open terminal and `cd` to that extracted folder, `sudo ./install.sh`
   - **or PPA alternative:**
   - `sudo apt-add-repository ppa:eosrei/fonts`
   - `sudo apt update`
   - `sudo apt install fonts-twemoji-svginot`

## Steam and Nvidia  
1. Install [steam from .deb](https://store.steampowered.com/about/) and run, configure basics.
2. Block and uninstall Nouveau open drivers, restart.
3. Install [Nvidia drivers](https://www.nvidia.com/Download/driverResults.aspx/137276/en-us) from website: `sudo sh nvidia-drivers.run`.
4. Restart, now can play. 

## Games no min when focus loss  
1. Edit `.profile` and add:
2. `export SDL_VIDEO_MINIMIZE_ON_FOCUS_LOSS=0`
3. Restart session

## Windows auto placement  
* `sudo apt install devilspie2`
* On Gnome Tweaks app can add `devilspie2.desktop` on `Startup Applications` ([ref.](https://github.com/thepante/setup/tree/master/.config/autostart))
* Using those basic scripts: [.lua files](https://github.com/thepante/setup/tree/master/.config/devilspie2)

## Secondary HDD and symlinks  
* Using `sudo apt install ntfs-config` 
* `sudo gedit /etc/fstab` - in the entry for `sda1` (second drive, storage one - my case): "defaults," argument to `defaults,exec,uid=1000,gid=1000` - if not, steam error with games download loop (`corrupt files` error [ref.](https://github.com/ValveSoftware/steam-for-linux/issues/4800#issuecomment-271763278)).
* ST3 local config and content linked to cloud service (_cloud.mail.ru_): `~/.config/sublime-text-3/Local` folder delete and replace with a link to synced folder to `/media/storage/Documentos/fpraffo@mail/SublimeText3/Local`.
* [Taskbook](https://github.com/klauscfhq/taskbook) folder `~/.taskbook` symlink to synced folder in `/media/storage/Documentos/fpraffo@mail/.taskbook` (_cloud.mail.ru_).
* Nautilus sidebar links to differents folders at storage drive.

## apt / Repositories  
Manually added repositories. Order without relevance.  
* [lp](https://launchpad.net/~no1wantdthisname/+archive/ubuntu/ppa) `sudo add-apt-repository ppa:no1wantdthisname/ppa`
* [lp](https://launchpad.net/~ubuntu-mozilla-daily/+archive/ubuntu/ppa) `sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa` 
* [lp](https://launchpad.net/~mc3man/+archive/ubuntu/mpv-tests) `sudo add-apt-repository ppa:mc3man/mpv-tests` 
* [lp](https://launchpad.net/~teejee2008/+archive/ubuntu/ppa) `sudo add-apt-repository ppa:teejee2008/ppa` 
* [lp](https://launchpad.net/%7Egraphics-drivers/+archive/ubuntu/ppa) `sudo add-apt-repository ppa:graphics-drivers/ppa`


## apt-fast and auto-accept second confirmation  
Only if I know that will depend on apt-fast under `apt` alias
* Install [apt-fast](https://github.com/ilikenwf/apt-fast) 
* And force yes the second confirmation:
  * ```sudo gedit /etc/apt/apt.conf.d/90forceyes```
  * Add and save: ```APT::Get::Assume-Yes "true";```  


With that I will do ```sudo apt install package``` and only have to confirm the apt-fast dialog. Have config ```apt``` as global alias (```alias -g apt='apt-fast'```)


## nodejs/npm  
* `sudo apt install nodejs` 
* `sudo apt install npm`
* `npm install -g npx`
* Create `~/.config/configstore` 
* Recheck permissions with user for: `~/.config/configstore`, `/usr/share/lib/node_modules`
* `bower init` and complete
* `bower install`
* Now can install eg:  
  - [transity](https://github.com/feramhq/transity) `npm install --global transity`  
  
## Fix boot speed  
Before and after test speed with `systemd-analize`
 1. Analize `systemd-analyze blame`
 2. Disable service slowing down - `sudo systemctl disable <service>`  

In my case `apt-daily.service` wrongly there, so I have to delay it adding:  
```bash
# apt-daily timer configuration override
[Timer]
OnBootSec=15min
OnUnitActiveSec=1d
AccuracySec=1h
RandomizedDelaySec=30min
```
Here `sudo EDITOR=gedit systemctl edit apt-daily.timer`  
  
**Also check startup application list, and delay some:**  
Launcher search for `Startup Applications`, edit those can delay with adding `sleep XX && ` in _Command_, before the actual one that launch the app.
* eg: `sleep 30 && /opt/urserver/urserver-start --no-manager --no-notify`

# zsh and spaceship prompt  
Steps
```bash
sudo apt install zsh git-core npm fonts-powerline
sudo chown -R $USER /usr/local 
npm install -g spaceship-prompt
chsh -s $(which zsh)
```
That's it! - Restart the terminal.  
And to prevent/fix that error about insecure directories:  
```bash
cd /usr/local/share/zsh
sudo chmod -R 755 ./site-functions
sudo chown -R root:root ./site-functions
```

# ! Mandatory ethernet drivers  
Check ethernet adapter driver version with `sudo ethtool -i enp3s0`  
If it is r8169, should install correct one, r8168 | [ref.](https://www.unixblogger.com/how-to-get-your-realtek-rtl8111rtl8168-working-updated-guide/)
```bash
sudo apt install r8168-dkms
```

# Enable share NTFS (secondary drive) over LAN, Samba  
If needed check with `system-config-samba` if the folder is correctly shared  
The _key_ issue was fixed with:  
* `sudo gedit /etc/samba/smb.conf`
* inside _[global]_ parameters need: `force user = pante`
* `sudo service smbd restart`
[ref.](https://forums.linuxmint.com/viewtopic.php?t=235233#p1250678)
