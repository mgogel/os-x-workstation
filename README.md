os-x-workstation
================

my os x workstation setup on mac os x mavericks. Macbook Pro if you care to know.

This is my personal macOS workspace setup for web development. If you’d like to install latest technologies and stay up to date, follow my guide and you will enjoy using your macOS computer more than ever.

This post will remain updated, as this guide is based on personal preference.

Tested and working on macOS High Sierra, version 10.13.5.

Introduction
I will assume that you have a clean macOS installation. Together, we will go through every step and installation process. There won’t be mistakes! If you’re not interested in learning, feel free to copy and paste the commands, as you go through this article.

Installations
The order is very important, so follow along every step, unless you know what you’re doing.

Xcode
Xcode is an integrated development environment for macOS containing a suite of software development tools developed by Apple for developing software for macOS, iOS, watchOS, and tvOS. 
Source: Wikipedia
We must begin with Xcode, but we do not need complete application.
Instead, we will install command line tools only.

Installation

xcode-select --install
Ta-da

We’re done with it. There’s nothing further for this one. Move on to the next…

Homebrew
Homebrew is a free and open-source software package management system that simplifies the installation of software on Apple’s macOS operating system.
Source: Wikipedia
Installation

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
Repositories

declare -a taps=(
  'buo/cask-upgrade'
  'caskroom/cask'
  'caskroom/versions'
  'homebrew/bundle'
  'homebrew/core'
)

for tap in "${taps[@]}"; do
  brew tap "$tap"
done
Upgrade and update

brew upgrade && brew update
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


HINT — to learn more about each command and its use, enter brew help [COMMAND] command, and it will display all details about specific command and each flag it has… If you’d like to learn more, see complete list of commands.

Homebrew Cask
Extends Homebrew and brings its elegance, simplicity, and speed to the installation and management of GUI macOS applications.
Source: GitHub Repository
Installation

brew install cask
List of applications and installation
I strongly suggest you to make your own personal list. Here’s mine…

declare -a cask_apps=(
  ‘1password’
  ‘adobe-creative-cloud’
  ‘alfred’
  ‘authy’
  ‘bartender’
  ‘droplr’
  ‘expressvpn’
  ‘flume’
  ‘gitkraken’
  ‘google-backup-and-sync’
  ‘google-chrome’
  ‘iterm2-nightly’
  ‘keepingyouawake’
  ‘postman’
  ‘screenflow’
  ‘sip’
  ‘skype’
  ‘slack’
  ‘sublime-text’
  ‘sequel-pro’
  ‘transmit’
)

for app in "${cask_apps[@]}"; do
  brew cask install "$app"
done
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


Mas CLI
A simple command line interface for the Mac App Store. Designed for scripting and automation. 
Source: GitHub Repository
Installation

brew install mas
List of applications and installation
I strongly suggest you to make your own personal list. Here’s mine…

declare -a mas_apps=(
  '824183456'   # Affinity Photo
  '824171161'   # Affinity Designer
  '918858936'   # Airmail 3
  '1091189122'  # Bear
  '736584830'   # Folx GO
  '775737590'   # iA Writer
  '441258766'   # Magnet
  '1063631769'  # Medis
  '967805235'   # Paste
  '583827028'   # WinZip
)

for app in "${mas_apps[@]}"; do
  mas install "$app"
done
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


Z Shell
A Unix shell that can be used as an interactive login shell and as a powerful command interpreter for shell scripting. 
Source: Wikipedia
Installation
We will install a couple of extensions along with a shell.

brew install zsh zsh-completions zsh-autosuggestions zsh-syntax-highlighting
Since we’re still at installing stage, it might be a good idea to install Oh My Zsh as well!

It comes bundled with a ton of helpful functions, helpers, plugins, themes, and a few things that make you shout…

sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
Configuration
Instead of editing the .zshrc file, we will make our own and then point it as a source to the main configuration file.

Create a file and open it in the Editor:

touch ~/.my-zshrc && bash -c 'exec env ${EDITOR:=nano} ~/.my-zshrc'

Copy/Paste the following content:

# Load extensions
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
# Activate plugins
plugins=(git zsh-completions)
# Custom vars
SPARK=$HOME/.spark-installer
COMPOSER=$HOME/.composer/vendor/bin
LOCAL_NODE_BIN=node_modules/.bin
# Custom paths
PATH=/usr/local/sbin:$PATH
PATH=$SPARK:$PATH
PATH=$COMPOSER:$PATH
PATH=$LOCAL_NODE_BIN:$PATH
# Set default editor
export EDITOR='subl -w'
# Load my aliases
if [ -f ~/.aliases ]; then
  . ~/.aliases
fi
# Load my functions
if [ -f ~/.functions ]; then
  . ~/.functions
fi
local ret_status="%(?:%{$fg_bold[green]%}△ :%{$fg_bold[red]%}▽ )"
PROMPT='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'
Append the source of our custom configuration file into the main Z Shell configuration file:

echo ". ~/.my-zshrc" >> "$HOME/.zshrc"
Aliases
This is completely optional. I use a bunch of aliases personally and I find them highly useful.

Create a file and open it in the Editor:

touch ~/.aliases && bash -c 'exec env ${EDITOR:=nano} ~/.aliases'
Copy/Paste the following content:

# Helpful
alias s='cd ~/Sites'
alias art='php artisan'
alias path='echo -e ${PATH//:/\\n}'
alias copy_ssh="pbcopy < $HOME/.ssh/id_rsa.pub"
alias reload="exec ${SHELL} -l"
alias afk="/System/Library/CoreServices/Menu\ Extras/User.menu/Contents/Resources/CGSession -suspend"
alias flush_dns="sudo killall -HUP mDNSResponder"
alias chdirs="find . -type d -exec chmod 755 {} \;"
alias chfiles="find . -type f -exec chmod 644 {} \;"
# Common files for editing
alias edit_hosts='subl /etc/hosts'
alias edit_httpd='subl /usr/local/etc/httpd/httpd.conf'
alias edit_vhosts='subl /usr/local/etc/httpd/extra/httpd-vhosts.conf'
alias edit_php='subl /usr/local/etc/php/7.2/php.ini'
# System
alias update='mas upgrade; brew cleanup; brew upgrade; brew update; brew cask cleanup; brew cu -a -y; composer global update; npm update -g; npm install npm@latest -g'
alias show_files='defaults write com.apple.finder AppleShowAllFiles -bool true && killall Finder'
alias hide_files='defaults write com.apple.finder AppleShowAllFiles -bool false && killall Finder'
alias show_desktop="defaults write com.apple.finder CreateDesktop -bool true && killall Finder"
alias hide_desktop="defaults write com.apple.finder CreateDesktop -bool false && killall Finder"
alias enable_gate="sudo spctl --master-enable"
alias disable_gate="sudo spctl --master-disable"
# IP
alias ip='dig +short myip.opendns.com @resolver1.opendns.com'
# NPM
alias nom='rm -rf node_modules/ && npm cache verify && npm install'
# Github
alias wip="git add .;git commit -m 'wip'"
alias nah='git reset --hard;git clean -df'
# Composer
alias dump='composer dump-autoload -o'
# Chrome
alias kill_chrome="ps ux | grep '[C]hrome Helper --type=renderer' | grep -v extension-process | tr -s ' ' | cut -d ' ' -f2 | xargs kill"
# Dummy
alias shrug="printf '¯\_(ツ)_/¯' | pbcopy"
alias flipt="printf '(╯°□°)╯︵ ┻━┻' | pbcopy"
alias fight="printf '(ง'̀-'́)ง' | pbcopy"
Functions
I do not use these as often, but they come handy from time to time.

Create a file and open it in the Editor:

touch ~/.functions && bash -c 'exec env ${EDITOR:=nano} ~/.functions'
Copy/Paste the following content:

# Make directory and enter it
function mkd () {
  mkdir -p "$@" && cd "$_";
}

# Copy website and its contents
function copy_website () {
  wget -e robots=off -p -k "$1"
}

# Extract most know archives with one command
extract () {
  if [ -f $1 ] ; then
    case $1 in
      *.tar.bz2)   tar xjf $1     ;;
      *.tar.gz)    tar xzf $1     ;;
      *.bz2)       bunzip2 $1     ;;
      *.rar)       unrar e $1     ;;
      *.gz)        gunzip $1      ;;
      *.tar)       tar xf $1      ;;
      *.tbz2)      tar xjf $1     ;;
      *.tgz)       tar xzf $1     ;;
      *.zip)       unzip $1       ;;
      *.Z)         uncompress $1  ;;
      *.7z)        7z x $1        ;;
      *)     echo "'$1' cannot be extracted via extract()" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}

# Determine size of a file or total size of a directory
function fs () {
  if du -b /dev/null > /dev/null 2>&1; then
    local arg=-sbh
  else
    local arg=-sh
  fi

  if [[ -n "$@" ]]; then
    du $arg -- "$@"
  else
    du $arg .[^.]* *
  fi
}
Ta-da

We’re done with it. There’s nothing further for this one. Move on to the next…

Sublime Text
A proprietary cross-platform source code editor with a Python application programming interface. 
Source: Wikipedia
Package Control
Here’s the Installation Code, but I think it is no longer required. 
Instead, open Sublime Text, press cmd + shift + p and type “Install”. It should show “Install Package Control”.

Packages
I have my curated list of packages, but of course I do not want to install them manually each time I reinstall my macOS.

To automatically install a list of packages, follow this path:

Preferences > Package Settings > Package Control > Settings — User
User Settings will open up… Replace the existing content with the following:

{
    "bootstrapped": true,
    "in_process_packages": [],
    "installed_packages":
    [
        "A File Icon",
        "AdvancedNewFile",
        "ApacheConf",
        "AutoFileName",
        "Babel",
        "BracketHighlighter",
        "ColorPicker",
        "DA UI",
        "DocBlockr",
        "EditorConfig",
        "Emmet",
        "GitGutter",
        "HyperClick",
        "JavaScript Completions",
        "Laravel Blade Highlighter",
        "MarkdownPreview",
        "Package Control",
        "Sass",
        "SFTP",
        "SnippetMaker",
        "SublimeLinter",
        "SublimeLinter-stylelint",
        "Terminal",
        "Vue Syntax Highlight"
    ]
}
Restart the application and give it some time to install specified packages.

Configuration
To setup preferences, first install a desired font. My personal preference is one of the following: Operator Mono or Fira Code.

After that, open Preferences > Settings or with a shortcut cmd + , and replace the contents in the right pane with the following. Make sure to replace font_face with the one that you installed on the system.

{
    "bold_folder_labels": true,
    "color_scheme": "Packages/Color Scheme - Default/Mariana.sublime-color-scheme",
    "copy_with_empty_selection": false,
    "create_window_at_startup": false,
    "detect_indentation": false,
    "drag_text": false,
    "enable_tab_scrolling": false,
    "ensure_newline_at_eof_on_save": true,
    "find_selected_text": true,
    "folder_exclude_patterns":
    [
        ".svn",
        ".git",
        ".hg",
        "CVS",
        "vendor",
        "node_modules"
    ],
    "font_face": "Operator Mono",
    "font_size": 16,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "indent_to_bracket": true,
    "line_padding_bottom": 6,
    "line_padding_top": 6,
    "margin": 8,
    "match_brackets_content": false,
    "match_selection": false,
    "match_tags": false,
    "open_files_in_new_window": false,
    "preview_on_click": false,
    "shift_tab_unindent": true,
    "show_full_path": false,
    "theme": "DA.sublime-theme",
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true
}
Ta-da

We’re done with it. There’s nothing further for this one. Move on to the next…

Apache
A free and open-source cross-platform web server, released under the terms of Apache License 2.0. 
Source: Wikipedia
Apache is already bundled in macOS. However, it is not the latest version and it’s always the best to keep all our workspace dependencies within Homebrew.

Across the configuration, you will need to replace all dvlden occurrences with your system username. Type whoami in the terminal to see your username, if you don’t know it already.

Stop and unload system-bundled version

sudo apachectl stop >/dev/null
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
Installation

brew install httpd
Configuration

Open configuration file in the Editor:

bash -c 'exec env ${EDITOR:=nano} /usr/local/etc/httpd/httpd.conf'
Here’s the table of contents. I hope I made it clear enough…


Configuration of dynamic virtual hosts

Open configuration file in the Editor:

bash -c 'exec env ${EDITOR:=nano} /usr/local/etc/httpd/extra/httpd-vhosts.conf
Replace the file contents with the following:

Define USER dvlden
Define PATH "/Users/${USER}/Sites"

<VirtualHost *:80>
    ServerName localhost
    DocumentRoot ${PATH}
</VirtualHost>

<VirtualHost *:80>
    ServerAlias *.test
    UseCanonicalName Off
    VirtualDocumentRoot "${PATH}/%1"
</VirtualHost>

<VirtualHost *:80>
    ServerAlias *.public
    UseCanonicalName Off
    VirtualDocumentRoot "${PATH}/%1/public"
</VirtualHost>
When we setup our DNSMasq below, we will be able to have automatic dynamic virtual hosts.

Every folder that we make in our ~/Sites directory, should be lowercase with hyphens, for readability. Think of the folder names as domain names, without TLD. Each folder will be automatically accessible. Use .test for static websites and use .public for dynamic sites. If you’re using an PHP Framework like Laravel, this will be very handy. It’s like Laravel Valet!

Restart

sudo apachectl -e info -k restart
Start a daemon

sudo brew services start httpd
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


DNS Masq
It provides Domain Name System forwarder, Dynamic Host Configuration Protocol server, router advertisement and network boot features for small computer networks, created as free software. 
Source: Wikipedia
We’re going to use this along with our dynamic virtual hosts configuration. This package will then redirect our specified TLD’s to a localhost.

Installation

brew install dnsmasq
Configuration

cat > "$(brew --prefix)/etc/dnsmasq.conf" <<EOF
address=/.test/127.0.0.1
address=/.public/127.0.0.1
EOF
Start a daemon

sudo brew services start dnsmasq
Add resolvers

sudo mkdir -p /etc/resolver
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/public'
Restart a daemon

sudo brew services restart dnsmasq
Test it

dig demo.test @127.0.0.1
You should be able to find the following section in your output.

;; ANSWER SECTION:
demo.test.   0 IN  A 127.0.0.1
PHP
A server-side scripting language designed for Web development, but also used as a general-purpose programming language. 
Source: Wikipedia
PHP is already bundled in macOS. However, it is not the latest version and it’s always the best to keep all our workspace dependencies within Homebrew.

Installation

brew install php
Installation of Composer
Since we’re using PHP, we are definitely going to need Composer.

brew install composer
Configuration

Open configuration file in the Editor:

bash -c 'exec env ${EDITOR:=nano} /usr/local/etc/php/7.2/php.ini'
Here’s the table of contents. I hope I made it clear enough…


Feel free to tweak the post and upload size for your needs…
You can find the timezones here: List of Supported Timezones

Ta-da

We’re done with it. There’s nothing further for this one. Move on to the next…

MySQL
An open-source relational database management system. Its name is a combination of “My”, the name of co-founder Michael Widenius’s daughter, and “SQL”, the abbreviation for Structured Query Language.
Source: Wikipedia
Installation

brew install mysql
Start a daemon

brew services start mysql
Configuration (optional)

By default MySQL is installed with a user root and no password. If you want to configure that, go ahead.

mysql_secure_installation
Node JS
An open-source, cross-platform JavaScript run-time environment that executes JavaScript code server-side. 
Source: Wikipedia
Installation

brew install node
NPM
A package manager for the JavaScript programming language. It is the default package manager for the JavaScript runtime environment Node.js. 
Source: Wikipedia
Node JS is bundled with NPM, so it will install it along with it… There’s nothing for us to do here, except to optionally install some global packages.

Global Packages Installation
If you are using Vue JS and perhaps want to use one epic package to keep your project dependencies up to date, check NPM Check Updates.

npm install -g npm-check @vue/cli
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


To learn more about each command and its use, enter npm help [COMMAND] command, and it will display all details about specific command and each flag it has... If you'd like to learn more, see complete list of commands.

Git
A version control system for tracking changes in computer files and coordinating work on those files among multiple people. 
Source: Wikipedia
Installation

brew install git
Configuration

Make sure to replace name and email with your personal details.

git config --global user.name "Nenad Novaković"
git config --global user.email "*.******@gmail.com"
git config --global core.editor "subl -n -w"
git config --global color.ui true
I’m using https as authentication, so I’ll add the following extra line to a configuration.

git config --global credential.helper osxkeychain
If you are using ssh authentication, you can set it up this way:

# Generate key
ssh-keygen -t rsa -C "*.******@gmail.com"

# Copy key
cat ~/.ssh/id_rsa.pub | pbcopy

# Add to Github
[Github SSH keys](https://github.com/settings/ssh)

# Test connection
ssh -T git@github.com

# > Hi dvlden! You've successfully authenticated, but GitHub does not provide shell access.
Ta-da

We’re done with it, but I will add a curated list of commands below. They are highly useful and you might need to memorize some of them for daily use.


To learn more about each command and its use, enter git help [COMMAND] command, and it will display all details about specific command and each flag it has... If you'd like to learn more, see complete list of commands.

Final Notes
This might be a good place for me to throw in my “.dot files”. 
If you’re interested in writing your own Shell to automate all of this process that I written and more, feel free to check this GitHub Repository.

Try it out if you want (on clean macOS installation), but I strongly suggest you to take a look at all of the files first and then maybe write your own.

Until next time
If you enjoyed this and learned something new, please share some love.
To do so, tap the clap icon 👏 or hold it for more claps! 👏👏👏

Much appreciated. You’re the best!
