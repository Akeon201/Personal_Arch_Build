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
sudo pacman -S git reflector ufw neofetch discord firefox-developer-edition tlp
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

### ufw firewall
```shell
sudo nano /etc/pacman.conf
```
