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
sudo pacman -S git reflector ufw neofetch discord firefox-developer-edition tlp yay
```

### Pacman
Parallel downloads, colors
```shell
sudo nano /etc/pacman.conf
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

### tlp power savings
```shell
sudo systemctl enable tlp.serive
sudo systemctl start tlp.serive
sudo systemctl mask systemd-rfkill.service
sudo systemctl mask systemd-rfkill.socket
sudo tlp start
```

### yay
https://aur.archlinux.org/yay.git
```shell
makepkg -s
pacman -U yay-XXXXXXXXXXXXXX.pkg.tar.zst
rm -rf yay/
```


### discord
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

### ufw firewall
```shell
sudo nano /etc/pacman.conf
```
