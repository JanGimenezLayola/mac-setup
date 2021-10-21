# Mac setup

This document details how I do the installation of my initial development environment of a new mac, I hope it helps and remember that this is my configuration and you should not follow it completely

If you detect an error or want to add something new, don't hesitate to open an `Issue`, a `PR` or [contact me](https://janlayola.cat).


The document assumes you are new to Mac, but can also be useful if you are reinstalling a system and need some reminder. The steps below were tested on macOS Big Sur (11.6), but should work for more recent versions as well.

- [System update](#system-update)
- [Security](#security)
- [System preferences](#system-preferences)
- [Software](#system-preferences)
  - [Package Manager - Homebrew](#package-manager---homebrew)
  - [Terminal - Iterm2](#terminal---iterm2)
  - [ZSH Framework - Oh-My-ZSH](#zsh-framework---oh-my-zsh)
  - [Version Control System - Git](#version-control-system---git)
  - [Node Version Manager - NVM](#node-version-manager---nvm)
  - [Package Manager - Yarn](#package-manager---yarn)
  - [Text Editor - VS Code](#text-editor---vs-code)
- [Recomendation](#recomendations)

---
## System update
First thing you need to do, on any OS actually, is update the system!

```
Apple icon (at menu bar) -> About This Mac -> Software Update... -> Update system if it's a new version available
```

## Security
I encourage you to encrypt your mac for greater security. Follow these simple steps and you will add a layer of security that will make it difficult to obtain your information if your device is lost or stolen.

```
In Apple icon (at menu bar) -> System Preferences:
   - Users & Groups: If you haven't already set a password for your user during the initial set up, you should do so now
   - Security & Privacy > FileVault: Make sure FileVault disk encryption is enabled
   - Security & Privacy > Firewall: Make sure Rirewall is turned off
   - Security & Privacy > General: Require password immediately after sleep or screen saver begins
```

## System preferences
If this is a new computer, there are a couple of tweaks I like to make to the `System Preferences`. Feel free to follow these, or to ignore them, depending on your personal preferences.

```
In Appple icon (at menu bar) -> System Preferences

   - General -> Appearance -> Dark
   - Trackpad -> All checked
   - Keyboard -> Key Repeat -> Fast
   - Keyboard -> Delay Until Repeat -> Short
   - Dock & Menu Bar -> Size small
   - Dock & Menu Bar -> Magnification -> unchecked
   - Dock & Menu Bar -> Position on screen -> Right
   - Dock & Menu Bar -> Animate opening applications -> unchecked
```
## Package Manager - Homebrew
[Homebrew](https://brew.sh/) is the easiest and most flexible way to install the UNIX tools Apple didn’t include with macOS. It can also install software not packaged for your Linux distribution to your home directory without requiring sudo.

### Installation
This script installs Homebrew to its preferred prefix (/usr/local for macOS Intel, /opt/homebrew for Apple Silicon) so that you don’t need sudo when you brew install. It is a careful script; it can be run even if you have stuff installed in the preferred prefix already. It tells you exactly what it will do before it does it too. You have to confirm everything it will do before it starts.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Once installation is complete, you can run the following command to make sure everything works:
```
brew doctor
```

### Usage
To install a package (or Formula in Homebrew vocabulary) simply type:
```
brew install <formula>
```
To see if any of your packages need to be updated:
```
brew outdated
```
To update a package:
```
brew upgrade <formula>
```
Homebrew keeps older versions of packages installed, in case you want to rollback. That rarely is necessary, so you can do some cleanup to get rid of those old versions:
```
brew cleanup
```
To see what you have installed (with their version numbers):
```
brew list --versions
```

## Terminal - Iterm2
[iTerm2](https://iterm2.com/) is a replacement for Terminal and the successor to iTerm. It works on Macs with macOS 10.14 or newer. iTerm2 brings the terminal into the modern age with features you never knew you always wanted.

### Installation
Download and install the lastest stable [iTerm2 release from oficial page](https://iterm2.com/downloads.html).

### Customization
I like to add some color, to make my terminal more clear and fun. My favorite color scheme is Atom One Dark and you can install running:
```
cd ~/Downloads
curl -o "Atom One Dark.itermcolors" https://raw.githubusercontent.com/nathanbuchar/atom-one-dark-terminal/master/scheme/iterm/One%20Dark.itermcolors
```
And you can add this theme to `Iterm2 Preferences` -> `Profiles` -> `Colors` -> `Color Presets...` (bottom right) -> `Import...` (select the Atom One Dark from Downloads folder) -> Open again `Color Presets...` and select `Atom One Dark`.

Close the terminal and opened again to see the changes.

## ZSH Framework - Oh-My-ZSH
[Oh My Zsh](https://ohmyz.sh/) is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout...

### Installation
Install Oh My Zsh by running:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
Adn you can upgrade by runnig:
```
omz update
```


## Version Control System - Git
[Git](https://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

### Installation
MacOS comes with a pre-installed version of Git, but we'll install our own through Homebrew to allow easy upgrades and not interfere with the system version. To do so, simply run:
```
brew install git
```
Then check the installation with:
```
which git
$ Output -> /usr/local/bin/git
```
If isn't display this output run:
```
echo alias git="/usr/local/bin/git" >> ~/.zshrc
```

### Setup
Download the [.gitconfig](https://raw.githubusercontent.com/JanGimenezLayola/mac-setup/master/.gitconfig) file to your home directory:
```
cd ~
curl -O https://raw.githubusercontent.com/JanGimenezLayola/mac-setup/master/.gitconfig
```
It will add some color to the status, branch, and diff Git commands, as well as a some aliases.

Then define your Git user (should be the same name and email you use for GitHub):
```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```
They will get added to your .gitconfig file. Check with:
```
cat ~/.gitconfig
```
On a Mac, it is important to remember to add .DS_Store (a hidden macOS system file that's put in folders) to your project .gitignore files. You also set up a global .gitignore file, located for instance in your home directory (but you'll want to make sure any collaborators also do it):
```
cd ~
curl -O https://raw.githubusercontent.com/JanGimenezLayola/mac-setup/master/.gitignore
git config --global core.excludesfile ~/.gitignore
```

## Node Version Manager - NVM
[Version manager for node.js](https://github.com/nvm-sh/nvm), designed to be installed per-user, and invoked per-shell. nvm works on any POSIX-compliant shell (sh, dash, ksh, zsh, bash), in particular on these platforms: unix, macOS, and windows WSL.

### Installation
Run the next script in your terminal:
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```
Running either of the above commands downloads a script and runs it. The script clones the nvm repository to ~/.nvm, and attempts to add the source lines from the snippet below to the correct profile file (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`).
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```
Verify the installation:
```
nvm --version
```

## Package Manager - Yarn
[Yarn](https://yarnpkg.com/) is a package manager for your code. It allows you to use and share (e.g. JavaScript) code with other developers from around the world. Yarn does this quickly, securely, and reliably so you don’t ever have to worry.

### Installation
Run in your terminal:
```
brew install yarn
```
Verify the installation:
```
yarn -v
```
## Text Editor - VS Code
[Visual Studio Code](https://code.visualstudio.com/) is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for `JavaScript`, `TypeScript` and `Node.js` and has a rich ecosystem of extensions for other languages (such as `C++`, `C#`, `Java`, `Python`, `PHP`, `Go`) and runtimes (such as `.NET` and `Unity`).

### Installation
Intall VSCode using HomeBrew:
```
brew install --cask visual-studio-code
```

### Setup
Just like the terminal, let's configure our editor a little. Go to Code > Preferences > Settings. In the very top-right of the interface you should see an icon with brackets that appeared { } (on hover, it should say "Open Settings (JSON)"). Click on it, and paste the following:

```
{
  "editor.tabSize": 2,
  "editor.rulers": [80],
  "files.trimTrailingWhitespace": true,
  "workbench.editor.enablePreview": false
}
```
Then save the setting with `CMD + S`
Open the command palette with `CMD + SHIFT + P` and search for `Shell Command: Install 'code' command in PATH`. Press enter, and you'll be able to run `code .` in the terminal to open a project.

Then go to the extensions menu (left bar menu) and install:
```
- Auto Close Tag
- Better Comments
- Bracket Pair Colorizer 2
- DotENV
- ESLint
- Prettier - Code Formatter
- indent-rainbow
- MarkDown Preview
- Path Intellisense
- Live Server
- Live Share
```

## Recomendations
Here is a list of apps that are essential in my daily work:
- [BitWarden](https://bitwarden.com/): Integrated open source password management solution for individuals, teams, and business organizations.
- [Slack](https://slack.com/): New way to communicate with your team. It's faster, better organized, and more secure than email.
- [Spectacle](https://www.spectacleapp.com/): Window Manager not being actively maintained anymore but light and simple.
- [Macs Fan Control](https://crystalidea.com/macs-fan-control/download): monitor and control almost any aspect of your computer's fans, with support for controlling fan speed, temperature sensors pane, menu-bar icon, and autostart with system option.
- [Dozer](https://github.com/Mortennn/Dozer): Hide menu bar icons to give your Mac a cleaner look.
