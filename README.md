# Mac OSX Manual Environment Set Up

## Introduction

This Readme is a step-by-step guide for how to set up your local environment
**on a Mac**. Please note that these instructions will not work for non-Mac
users. If you're on a Windows 10 machine, see the
[Windows Subsystem for Linux setup instructions][wsl]. If you're on an older
Windows machine, refer to the [Setting up Linux Virtual Box][linux]
instructions.

The following instructions are for macOS Catalina. If you are not on Catalina
but can upgrade, we recommend doing so before continuing. You can check your OS
version by clicking the apple menu in the top left and clicking 'About This
Mac'.

If you are unable to upgrade to Catalina, additional instructions are included
in the steps below.

## When to Move to a Local Environment

For online students, you should switch to a local environment after your
Ruby CLI project, or sooner if recommended by an instructor.

For in-person students, you should move to a local environment at the end of
Prework, before you start on campus.

## Step 0

First, if you've downloaded the Learn IDE, you will need to uninstall it. For
detailed instructions on how to properly uninstall the IDE, please read this
[Help Center article][uninstall IDE].

## Step By Step Instructions for Manual Installation


### Install Xcode Command Line Tools

Open up your terminal. You can do this by going to Applications > Utilities >
Terminal, or by using the quick launch (`cmd` + `space`) and just start typing
“Terminal”.

The first tools we're going to install are the Xcode command-line tools.
[Xcode][] is a suite of development tools from Apple, including tools for
building Mac and iPhone applications. For this course, we only need the Xcode
command-line tools, as many other tools rely on them. Run the following to
install the them:

```sh
xcode-select --install
```

You will be prompted to install Xcode Command-Line Tools. Agree and allow the
install to continue. You may need to provide your computer's password. To verify
the installation, you can run `xcode-select -p`. If you see
`/Applications/Xcode.app/Contents/Developer`, Xcode is ready to go.

### Install Homebrew

[Homebrew][] is a package manager for the Mac. It allows us to install a number
of things we will need. To install Homebrew, run the following:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

> **Note:** this is all one line in the terminal (even if it is broken up into
> two lines here in your browser).

You can verify that Homebrew is successfully installed by running `brew help`. If
your terminal outputs a list of `brew` commands, you're all set.

### Install Zsh

[Zsh][] is the new standard [shell][] for the macOS. To see if you're already
using Zsh, run the following:

```sh
echo $SHELL
```

If the terminal outputs `/bin/zsh`, Zsh is already installed. If something else
was output (like `/bin/bash`), run the following commands:

```sh
brew install zsh
chsh -s /bin/zsh
```

This will install Zsh and set it as the default shell.

Close your terminal and reopen. If you rerun `echo $SHELL`, the terminal should
output `/bin/zsh`.

### Install Git

Git generally comes pre-installed with most operating systems, but you can check
by running `git version` in the terminal. If this gives you an error or does not
come back with a version number, you'll need to install Git. you can get using
Homebrew:

```sh
brew install git
```

### Install Support Libraries

Next we need to add a few support libraries:

```sh
brew install gmp
```

and

```sh
brew install gnupg
```

> **Note:** If you get the following error: Warning: gnupg-1.4.19 already
> installed, it's just not linked, run:

```sh
brew link gnupg
```

### Install Ruby Version Manager

RVM is a tool that lets you run different versions of Ruby on your computer. If
one project you're working on works with Ruby version 2.3.3 and another needs
2.6.1, you can easily switch between the two versions when you switch between
projects. You can install RVM and set it up with the following commands:

* Run `curl -sSL https://get.rvm.io | bash`(make sure you do not use sudo)
* Run `source ~/.zprofile` (This reloads your terminal configuration file - similar to closing your terminal and opening it again)
* Run `rvm install 2.6.1`
* Run `rvm use 2.6.1 --default`
* Check that everything worked by running `ruby -v`. This should output the version of ruby you’re using (2.6.1)

If you want to see the list of versions that rvm has installed, you can run `rvm list`

### Install Some Ruby Gems

Ruby gems are pre-written, stand-alone, chunks of code that have been made
easily accessible to you. We'll use a lot of them soon, but for now, we should
get a few important ones.

* First, let's update our system gems by running `gem update --system`
* Next, install the Learn gems. Do this by running `gem install learn-co`
* Install the Bundler gem. This gem takes care of installing other gems you need
  for projects: `gem install bundler`
* Install Nokogiri with `gem install nokogiri` - Nokogiri is a gem to help parse
  HTML - useful when we want to scrape websites. If you encounter any errors
  while installing it, [check out the Nokogiri support docs][nokogiri support]
  for Mac OSX.

### Set Up the Learn gem

Now we need to set up the Learn gem. Type the following into your terminal:

```sh
learn whoami
```

This will prompt you to set up the Learn gem.

Note: When the gem asks you to go to learn.co/your-github-username, you must
fill your username into the URL and be logged in to be able to retrieve your
token. At the bottom of the your learn.co user page is an OAuth token. Paste
this token into the terminal when prompted to connect the Learn gem with your
account.

### Get a Text Editor

Modern text editors come with features that can be very helpful when starting
out as a programmer. We suggest [Visual Studio Code][VS Code]; follow the link to
download the macOS version.

After downloading and unzipping, make sure to move the Visual Studio Code app
from your Downloads folder to your Applications folder. Open finder and navigate
to Downloads (or wherever you save downloads). If you see Visual Studio Code
there, make sure to drag it over to Applications.

Once Visual Studio Code is in your Applications folder, launch the program and
type ⇧⌘P. Your Command Palette will open. In your Command Palette type  `>shell`
and select the option **"Shell Command: Install 'code' command in PATH"**. This
will allow you to use the `code` command in your terminal to open files in VS
Code.

Next, let's install Visual Studio Code as your default text editor in the
learn-config file. First, open the config file for Learn in a text editor (Let's
give Visual Studio Code a try!). If you successfully installed Visual Studio
Code and its shell commands, type `code ~/.learn-config` in your terminal. Your
`.learn-config` file should open in VSCode!

With the `.learn-config` file open, we can make a small change. Change default
editor from `subl` (or whatever it may be) to `code`.

> **Note:** [Atom][atom] is also a popular editor option. If you would prefer to
> use Atom over VS Code, you can. Just make sure that the `~/.learn-config` file
> has the correct editor. For VS Code, the file should include `:editor: code`,
> but for Atom users, this line should read: `:editor: atom`. If you have a
> different editor you prefer, you can set also it as the default learn editor
> in this file.

In `.learn-config`, you can also set the default location where Learn will save all your labs. By
default, the Learn directory is set to `/Users/<your-computer-username>/Development/code`.
If you want to store labs somewhere else, change this path to point to a different location. Make
sure the folders you point to exist!

So for instance, if we wanted to set our default lab location to 
`/Users/<your-computer-username>/Flatiron/code`, we would first make sure 
there the `Flatiron` folder exists as well as the subfolder `code`. We can do 
this in the terminal by changing our directory and making the folders:

```sh
cd  ~
mkdir Flatiron
mkdir Flatiron/code
```

This location setting is only triggered when using the `learn open` command to 
open a lesson. Some students have encountered Ruby Gem issues when
working in the default `Development/code` folder, so changing this now may be
helpful in preventing issues later on.

Save and close the `~/.learn-config` file.

If you would like to use the terminal built-in to VS Code, you may need to update
the settings. To do this, while in VS Code, press `command(⌘) + shift(⇧) + p` and
search for `settings.json`.

![VS Code settings.json](https://curriculum-content.s3.amazonaws.com/onboarding/vs%20code%20settings.png)

In this file, you should see opening and closing curly braces `{}` without anything
inside them. Add the following in between the braces:

```js
"terminal.integrated.env.osx": {
    "PATH": ""
}
```

If there are already items inside the curly braces, instead of erasing them, you can add a comma
after the last item and paste in the above setting on a new line. The file should look like this:

```js
{
  "terminal.integrated.env.osx": {
    "PATH": ""
  }
}
```

Or somethings similar to this:

```js
{
  "some.other.settings.present": true,
  "do.not.forget.the.end.comma": true,
  "terminal.integrated.env.osx": {
    "PATH": ""
  }
}
```

### Install SQLite

You’ll be using a couple of different databases as you move through the web
development track. The default database that Rails uses is SQLite.

To set up SQLite, run

```sh
brew install sqlite
```

### Install Postgres

We also frequently see that students want to deploy their apps to the free
hosting service [Heroku][]. To do this, though, you will need a different
relational database management system, PostgreSQL.

[Heroku]: https://www.heroku.com/

To install Postgres, run the following commands:

```sh
brew install postgres
brew services start postgresql
gem install pg
```

### Install Rails

Finally, Rails! The powerful Ruby web framework. We can install it with

```sh
gem install rails
```

### Install Node

Later on in the program, we'll want to run JavaScript just like we run ruby. On
the command line, the program that runs JavaScript files (the 'JavaScript
Runtime') is called Node.

To manage different versions of Node installed on our computer, we can use
javascript's equivalent of RVM - NVM. Let's get your node version manager
installed. Run the following in your terminal:

```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

Make sure you do not use `sudo`.

Next, run the following three commands:

```sh
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zprofile
echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"' >> ~/.zprofile
source ~/.zprofile
```

This sets NVM up to be accessible in your terminal. The last command refreshes
your shell so you won’t have to quit the terminal and open it again.

Finally, run the three following commands to install the latest version of Node:

```sh
nvm install node
nvm use node
nvm alias default node
```

### Install Chrome

Install [Google Chrome][] and [make Chrome your default browser][default browser].

[Google Chrome]: https://www.google.com/chrome/
[default browser]: https://support.google.com/chrome/answer/95417?co=GENIE.Platform%3DDesktop&hl=en

### Dotfiles

Configuration files are often prefixed with the '.' character, hiding them from
normal file explorer views - hence 'dotfiles'. Since the default configuration
of many of the tools we use is somewhat plain, it's helpful to add some
configuration on top.

These changes are optional, but recommended as they set up a variety of
convenient and ergonomic features to some of the most common tools you'll use -
irb , git , and zsh.

> **Note** If you ran the automatic install script earlier, your original
> dotfiles are saved as their filenames plus `.bac` at the end and there is no
> need to back the files as second time.

If you did not run the automatic install script, you'll first want to back up your
dotfiles before overwriting them. Run the following commands to do so:

```sh
mv ~/.irbrc{,.bak}
mv ~/.gitignore{,.bak}
mv ~/.zprofile{,.bak}
mv ~/.zshrc{,.bak}
mv ~/.gitconfig{,.bak}
```

> **Note:** If when you’re trying to back up a file, you get the error `No such
> file or directory` , don’t worry. This just means you didn’t have that file to
> start with, so there is nothing to back up.

After changing up the dot files, it is recommended you run
`rvm get stable --auto-dotfiles`. This will attempt to clear any potential
[PATH](https://en.wikipedia.org/wiki/PATH_(variable)) related issues.

#### IRB - `~/.irbrc`

To add some additional formating to IRB, run:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/irbrc" -o "$HOME/.irbrc"
```

#### Global List of Files for Git to Ignore - `~/.gitignore`

When code is sent from your local machine to GitHub, it is possible to
accidentally send files that aren't normally meant to be sent. To avoid this,
many Git repositories contain a `.gitignore` file, defining what files should be
ignored when code is being pushed to GitHub. We can also set a global `.gitignore`
file to use for all repositories.

GitHub maintains [a list of files they recommend][octocat gitignore]. These filenames and
a few others can be added to your own global `.gitignore` file by running:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/ubuntu-gitignore" -o "$HOME/.gitignore"
```

#### Setting up NVM and RVM and a Custom Prompt - `.zshrc`

The `~/.zshrc` file contains settings for terminal interaction. It runs every
time you open a new terminal window. NVM and RVM use this file to ensure they
are loaded and accessible when a terminal opens.

We can also use  `~/.zshrc` to customize what the prompt looks like. Running the
following will change your prompt and ensure NVM and RVM are set up to load correctly.

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/.zshrc" -o "$HOME/.zshrc"
source ~/.zshrc
```

Additional information on customizing your prompt can be found in the 
[optional installation instructions][optional].

[optional]: https://github.com/learn-co-curriculum/environment-mac-os-catalina-optional-setup

#### Helpful Zsh Profile Shortcuts - `.zprofile`

Your Zsh profile loads up every time you open a terminal window. Learn has a default
`.zprofile` that is designed to load up a bunch of shortcuts for you as well as make
sure that RVM loads up every time you open the terminal. To use this profile, run
the following:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/.zprofile" -o "$HOME/.zprofile"
```

We recommend you take a look at this file and even see if there are any
shortcuts of your own that you’d like to add!

#### Default Git Configuration File

To set up a default `.gitconfig`, run:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/gitconfig" -o "$HOME/.gitconfig"
```

### Configure Git

With Git installed, we now want to configure it using your personal account.
First, you need to let Git know who you are. You can do this by running:

```sh
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

Replace `"you@example.com"` with the email tied to your GitHub account and
`"Your Name"` with your GitHub username. Git will use this email and name as the
author for all the changes you make.

**Recommended:** While we're configuring GitHub, we should add a new SSH key.
Setting this key up will keep you from having to provide your username
and password whenever you use the terminal to interact with GitHub.

* First, check if you already have an SSH key by running
  `cat ~/.ssh/id_rsa.pub`. If the terminal prints out a long string of
  characters starting with `ssh-rsa`, you've already got a string and can skip
  the next bullet
* If the last command does not print anything, run `ssh-keygen` to create a
  new SSH key. You should be prompted to select a location and passphrase for
  your new key. Leave everything blank and press enter for the default
  location and no passphrase. If you’re asked if you want to overwrite, then
  you already have an SSH key and you do not want to overwrite it.

Run `cat ~/.ssh/id_rsa.pub` once more and copy the key that is printed out. Follow
the [instructions provided by GitHub][add ssh] and add this key to your GitHub account

#### Install Chrome

Install [Google Chrome][] and [make Chrome your default browser][default chrome].

### Install Slack

Install Slack for Mac and enable desktop notifications for Slack. One week
before your start date, you will receive an invitation to join the Flatiron
School workspace, `flatiron-school.slack.com`. You’ll also receive a welcome
email with information about channels you should join.

### Verify Installations

To verify that you've got everything installed, run the following command in
your terminal:

```sh
curl -so- https://raw.githubusercontent.com/learn-co-curriculum/flatiron-manual-setup-validator/master/manual-setup-check.sh | bash 2> /dev/null
```

This script will verify that everything you need is installed. If all checks pass,
you have completed the setup process and are ready to move on!

## Troubleshooting

Below are some options to try for specific issues.

### `learn whoami` Command Not Found / `learn` Produces `oj.bundle` Error

1.  Close out your terminal window, reopen it and try the `learn whoami` command
    again.

2.  Run the command `rvm list`. If you see a warning regarding `PATH`, try
    running the following first:

    ```sh
    rvm use 2.6.1
    rvm --default use 2.6.1
    ```

    Then reinstall the Learn gem and test it again with:

    ```sh
    gem install learn-co
    learn whoami
    ```

2.  If the `learn` command continues to fail, but RVM is working fine, try
    reinstalling RVM by first using the following command:

    ```sh
    rvm implode
    ```

    Then rerunning the automatic install script:

    ```sh
    curl -so- https://raw.githubusercontent.com/learn-co-curriculum/flatiron-manual-setup-validator/master/automatic-install.sh | bash 2> /dev/null
    ```

    Alternatively, you can try to just reinstall RVM with the following command:

    ```sh
    curl -sSL https://get.rvm.io | bash -s stable --ruby --auto-dotfiles
    ```

    Once RVM is installed, try reinstalling and testing the `learn-co` gem.

3.  If RVM is not found when you run `rvm list`, try installing RVM with the
    command shown above. You may get an error with further commands to try,
    including the following:

    ```sh
    gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

    or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
    ```

    After running the commands above, try reinstalling RVM again, then the
    `learn-co` gem.

If you are still unable to run `learn whoami`, try the following:

```sh
bundle clean --force
gem install learn-co
gem install bundler
```

This will clear out any gems that have already been installed. At the
moment, you will only need the `learn-co` and `bundler` gems, so this
reinstalls them.

## Resources

- [Uninstall the Learn IDE][uninstall IDE]
- [Adding SSH Key to GitHub][add ssh]
- [Nokogiri macOS Support][nokogiri support]
- [Visual Studio Code Download][VS Code]

[VS Code]: https://code.visualstudio.com/Download
[nokogiri support]: http://www.nokogiri.org/tutorials/installing_nokogiri.html#mac_os_x
[uninstall IDE]: http://help.learn.co/the-learn-ide/ide-settings/deleting-the-ide
[add ssh]: https://help.github.com/en/enterprise/2.15/user/articles/adding-a-new-ssh-key-to-your-github-account
[Oh My Zsh]: https://github.com/ohmyzsh/ohmyzsh
[atom]: https://atom.io/
[octocat gitignore]: https://gist.github.com/octocat/9257657
[Google Chrome]: https://www.google.com/chrome/
[default chrome]: https://support.google.com/chrome/answer/95417?co=GENIE.Platform%3DDesktop&hl=en
[Homebrew]: https://brew.sh/