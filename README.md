# Personal Arch Install

## Installation

### Arch iso guided installation
```shell
archinstall --script guided
```

### Subvolumes
One subvolume for root (@) and one for snapshots (.@snapshots). Varlog and tmp are created to disable Copy on Write on /var/log and /tmp.
```shell
/mnt/@
/mnt/@home
/mnt/@tmp
/mnt/@log
/mnt/@pkg
/mnt/@docker
/mnt/.@snapshots
```

### Subvolume Locations
```shell
/mnt  
/mnt/home
/mnt/tmp
/mnt/var/log
/mnt/var/cache/pacman/pkg/
/mnt/var/lib/docker
/mnt/.snapshots
```



## Post Installation
### Packages
```shell
sudo pacman -Syy
sudo pacman -Syu
sudo pacman -S git reflector ufw neofetch discord firefox-developer-edition tlp discord pacman-contrib openssh telegram-desktop zip unzip fish thunderbird gwenview kwalletmanager dnscrypt-proxy docker github-cli man-pages man-db cups
```

### BTRFS
The following are options you can add/remove.
You can switch out relatime for noatime to disable updating access time to files.
Adding nodatacow will disable copy-on-write for data (may benefit tmp and log).
Consider discard if you do not activate fstrim.timer (fstrim is 3 topics down). Discard might affect system performance, fstrim is recommended.
```shell
# Examples
/home           btrfs           rw,relatime,discard,ssd,space_cache=v2,subvolid=272,subvol=/@home
/tmp            btrfs           rw,compress=zstd,relatime,ssd,space_cache=v2,subvolid=260,subvol=/@tmp
```

List subvolumes

    sudo btrfs subvolume list /

### Secure Boot
Use sbctl to save time. Pacman hook auto setup for convenience.

    sbctl status
    sbctl create-keys
    sbctl enroll-keys -m
    sbctl sign -s /boot/EFI/BOOT/BOOTX64.EFI
    sbctl sign -s /boot/vmlinuz-linux
    sbctl sign -s -o /usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed /usr/lib/systemd/boot/efi/systemd-bootx64.efi
    sbctl status
    sbctl verify

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

### Telegram
Just configure settings.

### Chatgpt gui
https://aur.archlinux.org/chatgpt-desktop-bin.git
```shell
yay -S chatgpt-desktop-bin
```

### Vs code
https://aur.archlinux.org/visual-studio-code-bin.git
```shell
yay -S visual-studio-code-bin
```

### Dropbox
https://aur.archlinux.org/dropbox.git
```shell
yay -S dropbox
```

### Git
Account management for git
```shell
gh auth login
```

### Tor broswer
https://aur.archlinux.org/tor-browser.git
```shell
yay -S tor-browser
```

### Bluetooth
```shell
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

### Fish
Better terminal. Keep scripting to bash.
```shell
sudo pacman -S fish
```

### Timeshift
Configure in the gui. Auto snapshots may or may not work.
```shell
yay -S timeshift
# Following may or may not work. Broken on my arch install.
systemctl enable cronie.service
systemctl start cronie.service
```
Problem deleting entries or snapshots not listed?
    
    sudo btrfs subvolume delete --subvolid XXX /

### Firefox alt key fix
You can set the ui.key.menuAccessKeyFocuses pref to false on the about:config page to prevent opening the menu when holding down the Alt key. This may require to close and restart Firefox.

    ui.key.menuAccessKeyFocuses = false 
    
You can open the about:config page via the location/address bar. You can accept the warning and click "I accept the risk!" to continue.

### NetworkManager MAC configuration
For randomizing mac address.
    
    [device-mac-randomization]
    # "yes" is already the default for scanning
    wifi.scan-rand-mac-address=yes
    
    [connection-mac-randomization]
    # OPTIONS - random, stable
    # Stable means a random mac address will be generated and saved per connection.
    # Randomize MAC for every ethernet connection
    ethernet.cloned-mac-address=random
    # Generate a random MAC for each WiFi and associate the two permanently.
    wifi.cloned-mac-address=random

### Dnscrypt-proxy
    server_names = ['ahadns-doh-nl', 'adguard-dns-doh', 'controld-block-malware', 'dnsforge.de', 'adfilter-adl', 'mullvad-doh', 'doh-cleanbrowsing-security']
    log_files_max_age = 1 # max 1 day
    listen_addresses = ['127.0.0.1:53']
    ipv6_servers = true
    
    # Use servers implementing the DNSCrypt protocol
    dnscrypt_servers = false

    # Use servers implementing the DNS-over-HTTPS protocol
    doh_servers = true
    
    # Server must support DNS security extensions (DNSSEC)
    require_dnssec = true

    netprobe_timeout = -1
    ignore_system_dns = true
    bootstrap_resolvers = ['9.9.9.11:53', '1.1.1.1:53']

    # How long to keep backup files, in days
    log_files_max_age = 1

    # NETWORKMANAGER CONFIGURATION - Non optional if you installed with Archinstall guided installation
    Open /etc/NetworkManager/NetworkManager.conf
    dns=none
    rc-manager=unmanaged
    delete /etc/resolv.conf dns entries

    # OPTIONAL
    # Uncomment the following line to route all TCP connections to a local Tor node
    # Tor doesn't support UDP, so set `force_tcp` to `true` as well.
    proxy = 'socks5://127.0.0.1:9050'
    force_tcp = true

### cups
    ipp://XXX.XXX.XXX.XXX/ipp/port1



## TIPS
### Pacman and yay
```shell
# remove package
sudo pacman -Rsun <package>

# install package
sudo pacman -S <package>

# update download fresh package databases from the server (-yy to force a refresh even if up to date)
sudo pacman -Syy

# upgrade installed packages (-uu enables downgrades) (use y to be safe in case forgot above step)
sudo pacman -Syu

# install aur package
yay -S <package>
```

## Archives
```shell
# extract archive
tar -xvf <filename>.tar.bz2
```

## Ip and ports
```shell
# show ip
ip

# show active ports
ss
```

## Timeshift
If you encounter an error:
```shell
BTRFS error (device XXX): subvol'/@home' does not match subvolid XXX
```
You can find the new @home id with:
```shell
btrfs subvolume list /
```
Edit the subvolid at:
```shell
/etc/fstab
```
If still not working, try deleting the subvolid variable.
