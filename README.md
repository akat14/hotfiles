### Thanks for Visiting! 
This is my rice heavily based on https://github.com/syndrizzle/hotfiles/tree/bspwm

#### 2. ✨ The Bedazzle BSPWM ✨

### About:
* **Kitty** as the terminal.
* **Tokyo Night** as the color scheme.
* **BSPWM** as the window manager.
* **Picom (Fluffy Animations)** as the compositor.
* **EWW** as the widgets [Dashboard, Player and System Menu]
* **Rofi** as the application launcher.
* **SLiM** as the Display Manager.
* **Dunst** as the notification daemon.
* **Conky** as the desktop eyecandy.
* **jgmenu** as the desktop root menu.
* **Polybar** as the main bar.

---
## Installation:
Below are the installation steps to install the dotfiles of my setup. Click on a step to Expand it.
<b>1. Install Dependencies: </b>

Before we begin the installation, you need to create a `Downloads` folder in your `/home` folder if it is not there by default.
```bash
mkdir ~/Downloads
```
Install paru
```
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```
Then use it to install dependencies.
```bash
paru -S kitty polybar rofi bspwm-rounded-corners-git xdg-user-dirs nautilus xorg pavucontrol blueberry xfce4-power-manager feh lxappearance papirus-icon-theme file-roller gtk-engines gtk-engine-murrine neofetch imagemagick parcellite xclip maim gpick curl jq tint2 zsh moreutils recode dunst plank python-xdg redshift mate-polkit xfce4-settings mpv yaru-sound-theme fish alsa-utils slim xorg-xinit brightnessctl acpi mugshot playerctl python-pytz glava wmctrl i3lock-color jgmenu inter-font networkmanager-dmenu-git conky-lua bsp-layout zscroll noise-suppression-for-voice starship system76-power lsof gamemode lib32-gamemode xdo bluez bluez-utils bluez-libs bluez-tools
```

You also need `pylrc` which is a python module for handling the lyrics of song in the eww based player. You can skip this if you don't use spotify.
First install `pip`:
```bash
sudo pacman -S python-pip
```
Then:
```bash
sudo pip install pylrc
```
To install pylrc to your main `site-packages` folder.

Now you gotta install some dependencies which cannot be installed via the AUR helper/Pacman or it is better to install them this way:

#### 1. Eww
Elkowar's wacky widgets are the main widgets that we are gonna use in our system. It is a very essential dependency that you need.
First you need the nightly version of rust and also GTK3. A speedy way would be to directly install the binary package of rust nightly from the AUR using your favorite AUR helper:
```bash
paru -S rust-nightly-bin gtk3
```
Then we just need to run a few commands assuming you have `git` installed:
```bash
cd ~/Downloads
git clone https://github.com/elkowar/eww.git
cd eww
cargo build --release -j $(nproc)
cd target/release
sudo mv eww /usr/bin/eww
```
That installs eww to our root filesystem, which is then sourced from the `$PATH`.

#### 2. xqp
xqp comes from the author of `bspwm`.  It outputs the pointer ID under the window, basically, it is needed for the right click menu to function when clicking the root window in bspwm. The method of doing this was taken from [beyond9thousand](https://github.con/beyond9thousand)

NOTE: You need `base-devel` installed before this:
```bash
sudo pacman -S base-devel
```
Then you just gotta do:
```bash
cd ~/Downloads
git clone https://github.com/baskerville/xqp.git
cd xqp
make
sudo make install
```

#### 3. Picom Pijulius Fork
This picom fork has the best window animations you can get. For eyecandy we are using this fork, this isn't available in the AUR, so you need to install it manually:

First install all the dependencies required to build the compositor:
```bash
sudo pacman -S libconfig libev libxdg-basedir pcre pixman xcb-util-image xcb-util-renderutil hicolor-icon-theme libglvnd libx11 libxcb libxext libdbus asciidoc uthash
```
Then do the following:
```bash
cd ~/Downloads
git clone https://github.com/pijulius/picom.git
cd picom/
meson --buildtype=release . build --prefix=/usr -Dwith_docs=true
sudo ninja -C build install
```

Add your user to the ADM Group and start the following services:
```bash
sudo usermod -aG adm $USER
```

Start the system76-power service:
```bash
sudo systemctl enable --now com.system76.PowerDaemon
```

Bluetooth:
```bash
sudo systemctl enable bluetooth
```

With that, we have all the dependencies. We can move to the next part.
</details>
<details>
<summary><b>2. Installing GTK Theme:</b></summary>
To match with the current colorscheme, we are using the <a href="https://github.com/Fausto-Korpsvart/Tokyo-Night-GTK-Theme">Tokyo Night GTK Theme</a>

```bash
cd ~/Downloads
git clone https://github.com/Fausto-Korpsvart/Tokyo-Night-GTK-Theme.git
cd Tokyo-Night-GTK-Theme/
mv themes/Tokyonight-Dark-BL /usr/share/themes/
```
And that's it!
<b>3. Installing Dotfiles:</b>
The step we all have been waiting for.

Clone them and install:
```bash
cd ~/Downloads
git clone https://github.com/syndrizzle/hotfiles.git -b bspwm
cd hotfiles
cp -r .config .scripts .local .cache .wallpapers ~/
sudo cp -r usr/ /usr/
cp .xinitrc .gtkrc-2.0 ~/
```
Install Fonts:
Assuming you are already in the `hotfiles` folder
```bash
cd .fonts
mv * /usr/share/fonts
```
Move `slim.conf` and `environment` to it's location:
Again assuming you are in the `hotfiles` folder
```bash
cd etc/
mv slim.conf environment /etc/
```
Move items in `usr` folder to their respective places:
```bash
sudo mv usr/ /usr/
```
The usr folder contains the cursor theme and some executable scripts.

</details>

#### Miscllaneous:

<b>Spicetify Theme: </b>

Since we copied the dotfiles, we can apply the spicetify theme now.
First, install spicetify using:
```bash
paru -S spicetify-cli-git
```

Then, we need to give read and write access to our spotify folder for modifications:
```bash
sudo chmod a+wr /opt/spotify
sudo chmod a+wr /opt/spotify/Apps -R
```

After that we just need to run:

```bash
spicetify config current_theme Ziro
spicetify config color_scheme tokyonight
spicetify config extensions adblock.js
spicetify backup apply
```
This would install the spicetify theme to your Spotify.
