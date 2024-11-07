# Setup for Ubuntu 22.04 LTS

## Base

```sh
sudo apt install htop vim-gtk3 zenity gocryptfs
sudo ln -s /usr/bin/python3 /usr/local/bin/python
sudo apt install software-properties-common
```

## GPU

1. [Install amdgpu tools for my graphics card](https://www.amd.com/en/support/downloads/drivers.html/graphics/radeon-rx/radeon-rx-6000-series/amd-radeon-rx-6600-xt.html)
1. [Use amdgpu-install to install graphics set](https://amdgpu-install.readthedocs.io/en/latest/install-installing.html#installing-the-all-open-use-case)
    ```sh
    amdgpu-install --usecase=graphics,opencl --vulkan=amdvlk
    ```

Add user to video and render groups

```sh
sudo usermod -a -G render,video bill
```

## Browsers

Snap versions of apps do not integrate with 1password desktop app and have an irritating update process. Snap just automatically updates and insists that you have to restart your browser at an inconvenient time. You can pause snap updates for a short period but this is still frustrating.

### Firefox

```sh
sudo apt purge firefox

wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null
echo '
Package: *
Pin: origin packages.mozilla.org
Pin-Priority: 1000
' | sudo tee /etc/apt/preferences.d/mozilla
sudo apt update && sudo apt install firefox firefox-l10n-en-gb
```

Open about:config page and set:

* `browser.tabs.insertAfterCurrent` to `true`
* `full-screen-api.warning.timeout` to `0`

### Chromium

Remove Ubuntu's snap for Chromium:

```sh
sudo apt purge chromium-browser
sudo snap remove --purge chromium
```

Add Linux Mint chromium as a real apt package:

```sh
wget https://mirror.cov.ukservers.com/linuxmint/pool/main/l/linuxmint-keyring/linuxmint-keyring_2022.06.21_all.deb
sudo dpkg -i linuxmint-keyring_2022.06.21_all.deb
rm linuxmint-keyring_2022.06.21_all.deb

sudo tee /etc/apt/preferences.d/mint-pin >/dev/null <<'PREF'
# Upgrade only chromium from Linux Mint repository
Package: chromium*
Pin: release n=virginia
Pin-Priority: 500

# Do not prefer other packages from Linux Mint repository
Package: *
Pin: release n=virginia
Pin-Priority: -10
PREF

sudo tee /etc/apt/sources.list.d/mint.list >/dev/null <<'LIST'
deb https://mirrors.ukfast.co.uk/sites/linuxmint.com/packages/ virginia upstream
LIST

sudo apt update && sudo apt install chromium
```

## 1Password

https://1password.com/downloads/linux/

```sh
sudo apt install Downloads/1password-latest.deb
```

## Homesick

```sh
sudo apt install homesick git

ssh-keygen -t ed25519 -C 'my-comment'
```

Add public key to github

```sh
homesick clone git@github.com:biinari/config
homesick clone git@github.com:biinari/config-dir
homesick clone git@github.com:biinari/etc
homesick clone git@github.com:biinari/vim-config

homesick link config
homesick link vim-config
homesick link config-dir # skip xfce4 but use as reference for configs
homesick link etc

vim # install vundle
```

TODO: keep bin files in a homesick castle or similar

## XFCE

```sh
sudo apt install amixer # or alsa-utils
sudo apt install xarchiver xfce4-xkb-plugin xfce4-taskmanager

sudo apt install wmctrl xdotool
git clone https://github.com/calandoa/movescreen

sudo apt install simplescreenrecorder
```

TODO: Ensure we have bin files:
* `volume_master`
* `primary_monitor`

Use `~/.homesick/repos/config-dir/home/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-keyboard-shortcuts.xml` for inspiration to setup keyboard shortcuts with `xfce4-keyboard-settings` and for window manager settings with `xfwm4-settings`.

Setup keyboard panel plugin, using `Alt-Caps lock` for shortcut to switch layouts.

Setup Workspaces

Disable Removable Media autorun settings

## Useful apps

```sh
sudo apt install cheese galculator ristretto
sudo apt install mpv quodlibet smplayer vlc xfburn
sudo apt install gimp inkscape
sudo apt install libreoffice libreoffice-l10n-en-gb libreoffice-help-en-gb

sudo apt install openssh-server
```

## Development

```sh
sudo apt install docker.io docker-compose-v2
sudo apt install rbenv
sudo apt install shellcheck
sudo apt install nodejs npm
sudo apt install jq

sudo usermod -aG docker,disk,audio,video,users,systemd-journal,bluetooth,netdev,scanner bill
```

Install [go](https://golang.org/dl/)

Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Install [github CLI](https://cli.github.com/)

Install ruby-build:
```sh
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
sudo apt-get install autoconf patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev
```

Install rubies:
```sh
rbenv install 2.7.5
```

### PHP

Use [PHP PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php) to install a newer version if not supported in the main repository.

```sh
sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
```

```sh
VERSION=8.3

sudo apt install \
    php${VERSION}-cli \
    php${VERSION}-curl \
    php${VERSION}-mbstring \
    php${VERSION}-mysql \
    php${VERSION}-opcache \
    php${VERSION}-phpdbg \
    php${VERSION}-readline \
    php${VERSION}-xdebug \
    php${VERSION}-xml \
    php${VERSION}-zip \
    ;
```

## Youtube Download

```sh
sudo apt-get install yt-dlp
sudo apt-get install pandoc
```

## Remove annoyances

[Ubuntu 22.04 Annoyances](https://gist.github.com/jfeilbach/f4d0b19df82e04bea8f10cdd5945a4ff)

## For Linux Mint

```sh
sudo gtk-update-icon-cache /usr/share/icons/Mint-Y-Orange
sudo apt-get purge geoclue-2.0 caribou onboard orca

sudo apt-get install ntp
```
