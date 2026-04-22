---
title: Installing the beta build
parent: App
nav_order: 6
---

# Installing the beta build

The beta build of the `gitmastery` CLI (known as `gitmastery-beta`) is able to be used alongside the stable release. The installation rules are listed below:

## Debian/Ubuntu

```bash
echo "deb [trusted=yes] https://git-mastery.github.io/gitmastery-beta-apt-repo any main" | \
  sudo tee /etc/apt/sources.list.d/gitmastery-beta.list > /dev/null
sudo apt install software-properties-common
sudo add-apt-repository "deb https://git-mastery.github.io/gitmastery-beta-apt-repo any main"
sudo apt update
sudo apt-get install gitmastery-beta
```

## Arch

Use an AUR helper to install `gitmastery-beta-bin`. For example using yay:

```bash
yay -S gitmastery-beta-bin
```

Alternatively, you can build the `PKGBUILD` yourself following the [instructions on the Arch wiki](https://wiki.archlinux.org/title/Arch_User_Repository#Installing_and_upgrading_packages).

## macOS

```bash
brew tap git-mastery/gitmastery-beta
brew install gitmastery-beta
```

## Windows

Download the `gitmastery-beta.exe` file from the [release page](https://github.com/git-mastery/app/releases). Follow similar instructions to the [stable release installation](https://git-mastery.org/companion-app/index.html#preparation-recommended-install-and-configure-the-git-mastery-app) after.
