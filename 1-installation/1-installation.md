---
title: Installation
---

# Linux Install

## Automatic installation

There are user-made guides to install Tidal Cycles. Be sure to check out the following solutions:
- [Ansible method](https://club.tidalcycles.org/t/now-with-early-arch-manjaro-support-install-manage-upgrades-to-tidal-environment-with-a-single-command-on-ubuntu-debian-linux-mint-ansible-method/544): Ubuntu / debian / Mint / most debian-based distros
- [Yay](https://roosnaflak.com/tech-and-research/install-tidal-cycles-on-arch-linux/): installation on Arch / Manjaro.

## Manual installation

You need to install several components to get a complete Tidal Cycles system:
1. ([Git](https://git-scm.com/),
2. [Haskell](https://www.haskell.org/platform/),
3. [SuperCollider](http://supercollider.github.io/download),
4. [SuperDirt](https://github.com/musikinformatik/SuperDirt)), and
5. [an editor](TODO: link to editor page) to send Tidal commands to SuperDirt.

Hopefully, your Linux distribution makes the pre-requisites easily available to you via a package manager.

### Prerequisites

If your distribution uses the `apt` package manager (the Debian family of distros), you can install some of these dependencies using the following command:

```bash
sudo apt-get install build-essential cabal-install git jackd2
```

### SuperCollider

- **Ubuntu** / **Mint** / **Debian** :
  compiling Tidal Cycles from source is recommended to get the last version running. Packages are generally too old.
  Alternatively, try these scripts: [build-supercollider](https://github.com/lvm/build-supercollider).
- **Arch** / **Manjaro**:
  install via your [package manager](https://archlinux.org/packages/community/x86_64/supercollider/).

### SuperDirt

`SuperDirt` is the audio engine for Tidal Cycles — Tidal does not make sound without it.
SuperDirt also comes with the default audio samples used by Tidal.
To install `SuperDirt`, run the following in SuperCollider (either via the GUI, where you evaluate the expression by pressing Ctrl+Return, or via the `sclang` CLI):

```
Quarks.checkForUpdates({Quarks.install("SuperDirt", "v1.7.2"); thisProcess.recompile()})
```

It might take a while. You will know when the installation process is done by looking at the logs, where you should see:


```c
Installing SuperDirt
Installing Vowel
Vowel installed
Installing Dirt-Samples
Dirt-Samples installed
SuperDirt installed
compiling class library …
…

<!--T:41-->
*** Welcome to SuperCollider 3.10.0. *** For help press Ctrl-D.
```

<!-- TODO: add / move to editors page? -->
You can also install various plugins in your text editor to interact with SuperCollider without using the IDE ([Emacs](https://github.com/supercollider/scel), [Vim](https://github.com/supercollider/scvim), or [Atom](https://atom.io/packages/supercollider)).

### SC3 Plugins

`SC3Plugins` is a community-made extension for SuperCollider. Installing it is highly recommended: you won't be able to use the default synthesizers provided with Tidal Cycles without it. Please be sure to read [these instructions](https://supercollider.github.io/sc3-plugins/) to install the extension.


<!-- TODO: if on official page then remove here -->
(For Arch / Manjaro: there is an up-to-date package in the [Community repository](https://archlinux.org/packages/community/x86_64/sc3-plugins/).)

:::caution
If you installed SuperCollider using the [build-supercollider](https://github.com/lvm/build-supercollider) method, SC3Plugins is compiled and installed by the script.
:::

### TidalCycles

Open a Terminal. If you’re unsure how to open a terminal window in Linux, it varies according to distribution, but generally you can find `Terminal` in the menus.
Then type and run these two commands (ignoring any complaints that cabal has of 'legacy v1 style of usage'):

```bash
cabal update
cabal install tidal --lib
```

If you've never installed TidalCycles before, then the `cabal install tidal` step may take some time.
At the end of the command output, it should say `Completed tidal-x.x.x` (where x.x.x is the latest version number) without any errors.

# MacOS

## Automatic installation (script)

You can run the installation script by opening a terminal window, pasting in the following and pressing enter:

```bash
curl https://raw.githubusercontent.com/tidalcycles/tidal-bootstrap/master/tidal-bootstrap.command -sSf | sh
```

It will probably ask for your password. As you type, characters won't be shown on the screen. A lot of confusing info will scroll past. Just let it run until the end. Tidal should then be installed on your computer.

### What is the script doing with my computer?

The script installs the tools mentioned in TidalCycles [manual installation guide](../getting-started/Linux_installation).
In particular, the script checks if the following programs are installed on the system, and installs them if they are missing:

-   [SuperCollider](https://supercollider.github.io/) (and [SuperDirt](https://github.com/musikinformatik/SuperDirt))
-   [Atom](https://atom.io/) (and the [Tidal Cycles Plugin](https://atom.io/packages/tidalcycles))
-   [Haskell](https://www.haskell.org/) Language ([Ghcup](https://www.haskell.org/ghcup/))
-   The Tidal Pattern engine (Tidal Cycles itself)

## Manual installation

Before installing Tidal, make sure to install
1. [Haskell](https://www.haskell.org/ghcup/) (via [Ghcup](https://www.haskell.org/ghcup/)),
2. [SuperCollider](http://supercollider.github.io/downloads) with [SC3 Plugins](https://supercollider.github.io/sc3-plugins/),
3. [Git](https://git-scm.com/), and
4. a [text editor](TODO: link to text editor page) for interacting with TidalCycles.

### Installation process

#### Ghcup

In a terminal window, we will add the path to our GHC installation to a
file that is executed by our terminal every time it loads. For macOS 10.14 or before, the terminal uses bash, so the file we need
to modify is .bashrc:

```bash
. "$HOME/.ghcup/env"
echo '. $HOME/.ghcup/env' >> "$HOME/.bashrc"
```
For macOS10.15 Catalina, the terminal uses zsh, so the file we need to
modify is .zshrc:

```bash
. "$HOME/.ghcup/env"
echo '. $HOME/.ghcup/env' >> "$HOME/.zshrc"
```
After this, we will use cabal to first update it package directory, and then to install the TidalCycles library. We will also run these two commands every time we want to update our TidalCycles library to the latest version.

```bash
cabal update
cabal install tidal --lib
```
If you've never installed TidalCycles before, then the `cabal install tidal --lib` step may take some time. At the end of the command output, it should say `Installed tidal-x.x.x` (where x.x.x is the latest version number) without any errors.

#### SuperDirt

<!-- TODO: refactor so this isn't repeated -->

Start your freshly installed version of SuperCollider. Paste the following line of code in the text editor you see and press Cmd+Return to evaluate :

```
Quarks.checkForUpdates({Quarks.install("SuperDirt", "v1.7.2"); thisProcess.recompile()})
```

Run the code by clicking on it, to make sure the cursor is on this line,
then hold down Shift and press Enter.

It'll take a while to install. You'll see something like the following:

```c
Installing SuperDirt
Installing Vowel
Vowel installed
Installing Dirt-Samples
Dirt-Samples installed
SuperDirt installed
compiling class library...
...
(then some blah blah, and finally, something like:)
...

<!--T:31-->
*** Welcome to SuperCollider 3.11.2. *** For help press Ctrl-D.
```


# Windows installation

## Automatic installation

There is an automatic installer for Tidal Cycles for Windows. It will install everything you need, including the required dependencies ([Git](https://git-scm.com/), [Haskell](https://www.haskell.org/ghcup/), [Atom Editor](https://atom.io/), [SuperCollider](http://supercollider.github.io/download), [SuperDirt](https://github.com/musikinformatik/SuperDirt)). The installer assumes that these components aren't installed already. If they are, you might be better off installing all the rest by hand

### Installation procedure

Installation is a three step process. It is mostly automated, but expect Windows to give a few security pop-ups for you to accept.
Windows 7 users: please see the specific section at the end of this page.

#### - Starting powershell as administrator

- Windows 10 - Hold down the windows key and press 'x', then choose Windows PowerShell (admin) in the menu that pops up.
- Windows 7 - Click the start button, type `powershell`, then click with the right mouse button and choose **Run as Administrator**.

#### Installing Chocolatey: the package manager

The [Chocolatey](https://chocolatey.org/) package manager is required. If you haven't installed it previously, you can get it by running this command:

```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

#### Installing Tidal Cycles

Run the following command to install Tidal Cycles using Chocolatey:

```shell 
choco install tidalcycles 
```

*Note:* We are still working on the automatic installer. A lot of confusing information will scroll past. Please ignore messages about restarting Powershell. Just let the process run to the end.

## Manual installation

You might prefer to install the different components of Tidal Cycles by hand. This is the recommand way for users who already installed some of the components ([Git](https://git-scm.com/), [Haskell](https://www.haskell.org/ghcup/), [Atom Editor](https://atom.io/), [SuperCollider](http://supercollider.github.io/download), [SuperDirt](https://github.com/musikinformatik/SuperDirt)). All these components are needed to install Tidal Cycles.

### SC3 Plugins

[SC3 Plugins](https://supercollider.github.io/sc3-plugins/) is needed if you want to use any of the synthesizers included with Tidal Cycles. Most of the examples in the documentation will still work, but your intallation will likely be incomplete without it. You can skip the installation but be sure to come back later to install it if you want.

### SuperDirt

<!-- TODO: refactor so this is not repeated again -->

SuperDirt is the audio engine used by Tidal. Without it, Tidal Cycles will not emit any sound and will not be able to communicate through OSC or MIDI with other synthesizers. To install it, open SuperCollider and run the following command in the interactive editor (press Ctrl+Return to evaluate):

```
Quarks.checkForUpdates({Quarks.install("SuperDirt", "v1.7.2"); thisProcess.recompile()})
```

The installation will take a little while. You will know when the installation process is done by looking at the logs window. It will likely print something like the following:

```c 
Installing SuperDirt
Installing Vowel
Vowel installed
Installing Dirt-Samples
Dirt-Samples installed
SuperDirt installed
compiling class library...
...
(then some blah blah, and finally, something like:)
...

<!--T:28-->
*** Welcome to SuperCollider 3.12.1. *** For help press Ctrl-D.
```


### Tidal Cycles

You will need [Haskell](https://www.haskell.org/ghcup/) (Ghcup) to install Tidal Cycles. If you just installed it or already got it installed, open `PowerShell` in **administrator mode** (see above). Enter the following commands:

```shell
cabal v1-update
cabal v1-install tidal
```

The last command might take some time to complete. Be patient, and everything will be alright :smile:. 


-----

## Note for Windows 7 users

If you are using Windows 7, some extra preparation is required before installing everything else:

1.  Make sure Windows 7 is up to date with the latest patches.
2.  Install/upgrade to .NET 4.5. You can [download it here](https://www.microsoft.com/en-gb/download/details.aspx?id=30653).
3.  Install Visual C++ redistributable. You can [download it here](https://support.microsoft.com/en-gb/help/2977003/the-latest-supported-visual-c-downloads)

-----

## I've installed Tidal Cycles. What's next?

Look at the sidebar. You will see a list of text editors you can install to interact with Tidal and start playing :smile:.
