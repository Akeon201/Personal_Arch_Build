# Personal Arch Install

## Arch iso guided installation
```shell
archinstall --script guided
```

## Post Installation
### Packages
```shell
sudo pacman -Syu
sudo pacman -Syy
sudo pacman -S git reflector ufw neofetch discord firefox-developer-edition tlp discord pacman-contrib powertop
```

### Pacman
Parallel downloads, colors
```shell
sudo nano /etc/pacman.conf
```

### Paccache
Keeps most recent 3 cache versions
```shell
sudo pacman -S pacman-contrib
paccache -r
sudo systemctl enable paccache.timer
sudo systemctl start paccache.timer
```

### Fstrimer timer
Might already be enabled, check with 'systemctl list-timers'
```shell
sudo systemctl enable fstrimer.timer
sudo systemctl start fstrimer.timer
```

### Reflector
Take command and put into config file for automatic updating.
```shell
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
sudo reflector --country "United States" --verbose --age 24 --protocol https --sort --save /etc/pacman.d/mirrorlist
sudo nano /etc/xdg/reflector/reflector.conf
systemctl enable reflector.timer
systemctl start reflector.timer
```

### Tlp power savings
```shell
sudo systemctl enable tlp.serive
sudo systemctl start tlp.serive
sudo systemctl mask systemd-rfkill.service
sudo systemctl mask systemd-rfkill.socket
sudo tlp start
```

### Yay
https://aur.archlinux.org/yay.git
```shell
makepkg -s
pacman -U yay-XXXXXXXXXXXXXX.pkg.tar.zst
rm -rf yay/
```


### Discord
To disable the update check, add the following to ~/.config/discord/settings.json: 
```shell
~/.config/discord/settings.json
"SKIP_HOST_UPDATE": true
```

Note that you will need to add an extra comma after the "WINDOW_BOUND" array due to JSON requirements, i.e.: 
```shell
# If settings.json file is not generated, start discord, resize window, close.
{ 
  "IS_MAXIMIZED": true,
  "IS_MINIMIZED": false,
  "WINDOW_BOUNDS": {
    "x": 2240,
    "y": 219,
    "width": 1280,
    "height": 720
  },
"SKIP_HOST_UPDATE": true
}
```

### Ufw firewall
Just configure within kde settings.

### C  pu auto-frequency
```shell
```

### Powertop
```shell
```
