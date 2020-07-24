# Mac OSX Local Environment Setup Instructions

## Introduction

This Readme is a step-by-step guide for how to set up your local environment
**on a Mac**. Please note that these instructions will not work for non-Mac
users. If you're on a Windows 10 machine, see the
[Windows Subsystem for Linux setup instructions][wsl]. If you're on an older
Windows machine, refer to the [Setting up Linux Virtual Box][linux]
instructions.

The following instructions are for macOS Catalina. If you are not on Catalina
but can upgrade, we recommend doing so _after_ following the instructions below.
Additional instructions are included in the steps below for non-Catalina users.

You can check your OS version by clicking the apple menu in the top left and
clicking 'About This Mac'.

## When to Move to a Local Environment

To ensure you can access and complete the assignments in this course,
switching to a local environment at the start of the course is best.

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
install them:

```sh
xcode-select --install
```

You will be prompted to install Xcode Command-Line Tools. Agree and allow the
install to continue. You may need to provide your computer's password. 

> **Important:** If the Xcode Command-Line Tools aren't installed, you may encounter
> errors later on when working with gems like `sqlite3`. To double check that everything is installed,
> rerun the `xcode-select --install` command. If everything is installed, you should see this error:
>
> ```sh
> xcode-select: error: command line tools are already installed, use "Software Update" to install updates
> ```
>
> If you receive this error, you are good to continue!


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

### Install the GMP and GnuPG Packages

Before continuing further, we need to install some libraries that other tools
rely on, [GMP][] and [GnuPG][]:

```sh
brew install gmp
brew install gnupg
```

> **Note:** If you get this error: `Warning: gnupg-1.4.19 already installed`,
> GnuPG is installed, but it may not be linked properly. To fix, run:
>
> ```sh
> brew link gnupg
> ```

### Install RVM

[RVM][] is a tool that lets you run different versions of Ruby on your computer.
If one project you're working on works with Ruby version 2.3.3 and another needs
2.6.1, you can easily switch between the two versions when you switch between
projects.

The following command downloads encryption keys we need to install RVM:

```sh
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

If you get an error that gpg2 is not found, try with this command instead:

```sh
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

> **Note:** If _neither_ of the above commands work, or you receive an error stating 
> `keyserver receive failed: No route to host`, try running one of the following commands:
> 
> ```sh
> command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
> ```
> 
> ```sh
> command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
> ```

Once the encryption keys are downloaded, use the following command to download RVM:

```sh
curl -sSL https://get.rvm.io | bash -s stable --ruby --auto-dotfiles
```

When RVM is installed, run `rvm reload` **or** close and reopen your terminal to make sure RVM is fully
loaded. Next, we will install the Ruby version we'll be using and set it as the default:

```sh
rvm install 2.6.1
rvm use 2.6.1 --default
```

To check that everything worked, run `rvm list`. You should see `=* ruby-2.6.1`
listed, indicating that `2.6.1` is installed and set as the default version for
Ruby. You can also run `ruby -v`, which should show that Ruby `2.6.1` is the
current version of Ruby being used.

[RVM]: https://rvm.io/

Before continuing, close and reopen your terminal and run `rvm list` one more
time to make sure everything is working.

> **Note:** If you see an error or warning when running `rvm list`, we recommend
> following the troubleshooting steps at the end of this lesson before
> continuing.

### Install Some Ruby Gems

Ruby gems are pre-written, stand-alone, chunks of code that have been made
easily accessible to you. We'll use a lot of them soon, but for now, we should
get a few important ones.

* First, let's update our system gems by running `gem update --system`
* Next, install the Learn gem. Do this by running `gem install learn-co`. This
  gem gives us access to `learn` and `learn submit` commands for labs.
* Install the Bundler gem with `gem install bundler`. This gem takes care of
  installing other gems you will need for projects.
* Install Nokogiri with `gem install nokogiri` - Nokogiri is a gem to help parse
  HTML - useful when we want to scrape websites. If you encounter any errors
  while installing it, [check out the Nokogiri support docs][nokogiri support]
  for Mac OSX.
* Install Pry with `gem install pry`. You may have already used Pry during the
  Prework while using the in-browser IDE. Installing Pry here will make it available
  for projects in your local environment.
* Install Ruby on Rails with `gem install rails` - [Rails][] is used for
  building out full web applications. We will learn much more about Rails soon.

Before continuing, check to make sure the `learn-co` gem was properly installed. Run
`learn` in the terminal. If you're in your home directory (`cd ~`), running `learn` should
produce the following message:

```sh
You don't appear to be in a Learn lesson's directory. Please cd to an appropriate directory and try again.
```

If you receive an error message, try reinstalling with `gem install learn-co` again, close
and reopen the terminal and try the `learn` command a second time.

### Install Git

Git generally comes pre-installed with most operating systems, but you can check
by running `git version` in the terminal. If this gives you an error or does not
come back with a version number, you'll need to install Git. you can get it
using Homebrew:

```sh
brew install git
```

### Set Up the Learn gem

Now we need to set up the Learn gem. Type the following into your terminal:

```sh
learn whoami
```

This will prompt you to set up the Learn gem using a token provided on
`learn.co`.

* If you have connected your Github account to your Learn account, navigate to
  `learn.co/your_github_username`. The OAuth token is at the bottom of the page.
* If you have not connected your Github account: Go to [your profile][] > Learn
  Settings > Public Profile. A URL should be listed under **Username**. Navigate
  to this URL and scroll to the bottom of the page. The OAuth token is at the
  bottom of the page.

> **Note:** At the end of this lesson is additional troubleshooting information.
> If you receive an error when running `learn whoami`, please try the steps
> listed there.

### Get a Text Editor

Get a Text Editor. We suggest [Visual Studio Code][VS Code]; follow the link to
download the macOS version.

After downloading and unzipping, make sure to move the Visual Studio Code app
from your Downloads folder to your Applications folder. Open Finder and navigate
to Downloads (or wherever you save downloads). If you see Visual Studio Code
there, drag it over to Applications.

Once Visual Studio Code is in your Applications folder, launch the program and
type `command(⌘) + shift(⇧) + p`, and your **Command Palette** will open. In
your **Command Palette**, type `>shell command`. Select "Shell Command: Install
'code' command in PATH"

![VS Code Add to Path](https://curriculum-content.s3.amazonaws.com/onboarding/vscode%20path.png)

Next, let's install Visual Studio Code as your default text editor in the
`.learn-config` file. First, open the config file for Learn in a text editor (let's
give Visual Studio Code a try!). If you successfully installed Visual Studio
Code and its shell commands, type `code ~/.learn-config` in your terminal. Your
`.learn-config` file should open in VSCode!

If your `.learn-config` is blank, or is missing, use the following template. Make your
changes and save it in your root (`~`) directory.

```sh
---
:learn_directory: "/Users/<your-computer-username>/Flatiron/code"
:editor: 'code'
```

Change default editor from `subl` (or `atom`) to `code`.

> **Note:** [Atom][atom] is also a popular editor option. If you would prefer to
> use Atom over VS Code, you can. Just make sure that the `~/.learn-config` file
> has the correct editor. For VS Code, the file should include `:editor: code`,
> but for Atom users, this line should read: `:editor: atom`. If you have a
> different editor you prefer, you can set it as the default learn editor
> in this file.

In `.learn-config`, you can also set the default location where Learn will save
all your labs. By default, the Learn directory is set to
`/Users/<your-computer-username>/Development/code`. However, some students have
reported issues using Ruby Gems when working in this folder. To avoid this
potential problem, let's make a new folder and set it as the default location.
instead of `Development/code`, lets make a `Flatiron/code` folder:

```sh
cd  ~
mkdir Flatiron
mkdir Flatiron/code
```

Then, in `.learn-config`, change the Learn directory to
`/Users/<your-computer-username>/Flatiron/code`. If you want to store labs
somewhere else, change this path to point to the desired location. Make sure the
folders you point to exist!

This location setting is only triggered when using the `learn open` command to
open a lesson. Save and close the `~/.learn-config` file.

#### Optional VS Code Terminal Setup

If you would like to use the terminal built into VS Code, you may need to update
the settings. If you intend to use your regular terminal, you do not need to
complete this step.

To update VS Code's terminal settings, while in VS Code, press
`command(⌘) + shift(⇧) + p` and search for `settings.json`.

![VS Code settings.json](https://curriculum-content.s3.amazonaws.com/onboarding/vs%20code%20settings.png)

In this file, you should see opening and closing curly braces `{}` without
anything inside them. Add the following in between the braces:

```js
"terminal.integrated.env.osx": {
    "PATH": ""
}
```

If there are already items inside the curly braces, instead of erasing them, you
can add a comma after the last item and paste in the above setting on a new
line. The file should look like this:

```js
{
  "terminal.integrated.env.osx": {
    "PATH": ""
  }
}
```

Or something similar to this:

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

To set up SQLite, run:

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

### Install Node

Later on in the program, we'll want to run JavaScript just like we run Ruby. On
the command line, the program that runs JavaScript files (the 'JavaScript
Runtime') is called Node.

To manage different versions of Node installed on our computer, we can use
JavaScript's equivalent of RVM - NVM. Let's get your Node Version Manager
installed. Run the following in your terminal:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

Make sure you do not use `sudo`.

Next, run the following commands:

```sh
echo "$(echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"' | cat  - ~/.zshrc)" > ~/.zshrc
echo "$(echo 'export NVM_DIR="$HOME/.nvm"' | cat - ~/.zshrc)" >> ~/.zshrc
source ~/.zshrc
```

This sets NVM up to be accessible in your terminal whenever you open it. The
last command refreshes your shell so you won’t have to quit the terminal and
open it again.

Finally, run the three following commands to install the latest version of Node:

```sh
nvm install node
nvm use node
nvm alias default node
```

After installing, you can verify everything is working by running `nvm list`. If
NVM has installed correctly, this will output the existing versions of Node and 
indicate which version is currently set to default.

### Dotfiles

Configuration files are often prefixed with the '.' character, hiding them from
normal file explorer views - hence 'dotfiles'. Since the default configuration
of many of the tools we use is somewhat plain, it's helpful to add some
configuration on top.

These dotfiles set up a variety of convenient features to some of the most
common tools you'll use - Zsh and Git.

You'll first want to back up your dotfiles before overwriting them. Run the
following commands to do so:

```sh
mv ~/.gitignore{,.bak}
mv ~/.zprofile{,.bak}
mv ~/.gitconfig{,.bak}
```

> **Note:** If when you’re trying to back up a file, you get the error `No such
> file or directory`, don’t worry. This just means you didn’t have that file to
> start with, so there is nothing to back up.

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

#### Helpful Zsh Shortcuts - `.zprofile`

Your Zsh profile loads up every time you open a terminal window. Learn has a default
`.zprofile` that is designed to load up a bunch of shortcuts for you as well as make
sure that RVM loads up every time you open the terminal. To use this profile, run
the following:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/.zprofile" -o "$HOME/.zprofile"
source ~/.zprofile
```

We recommend you take a look at this file and see if there are any
shortcuts of your own that you’d like to add!

#### Set up the Default Git Configuration File

In the next step, we'll configure Git, but before we do, we will set up a default
configuration file called `.gitconfig`. To get the default `.gitconfig` file,
run:

```sh
curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/gitconfig" -o "$HOME/.gitconfig"
```

> **Note:** After changing up the dotfiles, it is recommended you run:
>
> ```sh
> rvm get stable --auto-dotfiles
> ```
>
> This will attempt to clear any potential [PATH][] related issues.

### Configure Git

With Git installed, we now want to configure it using your own account.
First, you need to let Git know who you are. You can do this by running:

```sh
git config --global user.email "you@example.com"
git config --global user.name "Your Username"
```

Replace `"you@example.com"` with the email tied to your GitHub account and
`"Your Username"` with your GitHub username. Git will use this email and name as the
author for all the changes you make.

**IMPORTANT:** While we're configuring GitHub, we should add a new SSH key.
Setting this key up will keep you from having to provide your username
and password whenever you use the terminal to interact with GitHub.

* First, check if you already have an SSH key by running
  `cat ~/.ssh/id_rsa.pub`. If the terminal prints out a long string of
  characters starting with `ssh-rsa`, you've already got a key and can skip
  the next bullet
* If the last command does not print anything, run `ssh-keygen` to create a
  new SSH key. You should be prompted to select a location and passphrase for
  your new key. Leave everything blank and press enter for the default
  location and no passphrase. If you’re asked if you want to overwrite, then
  you already have an SSH key, and you do not want to overwrite it.

Run `cat ~/.ssh/id_rsa.pub` once more and copy the key that is printed out.
**Follow the [instructions provided by GitHub][add ssh] and add this key to your
GitHub account**

### Install Chrome

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

### RVM Is Producing Errors or Warnings

1. Close your terminal, reopen it, and try the `rvm list` command.

2. If you see a warning regarding the `PATH`, try running the following
   first:

   ```sh
   rvm use 2.6.1
   rvm --default use 2.6.1
   ```

   Close and reopen the terminal again, and rerun `rvm list`.

3. If RVM is not found when you run `rvm list`, try reinstalling RVM:

    ```sh
    curl -sSL https://get.rvm.io | bash -s stable --ruby --auto-dotfiles
    ```

    You may get an error regarding keys with further
    commands to try, including the following:

    ```sh
    gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

    or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
    ```

   Try each of these, followed by the previous `curl` command to install RVM.

4. If RVM is found but continues to produce errors, try uninstalling with:

   ```sh
   rvm implode
   ```

   This will remove RVM entirely. Follow the instructions in Step 3 to reinstall RVM.

### `learn whoami` Command Not Found / `learn` Produces `oj.bundle` Error

1. Close your terminal window, reopen it, and try the `learn whoami` command
   again.

2. Run the command `rvm list`. If RVM is not found, follow the steps in the
   previous troubleshooting section on installing RVM. If you see a warning
   regarding `PATH`, try running the following first:

   ```sh
   rvm use 2.6.1
   rvm --default use 2.6.1
   ```

   Then reinstall the Learn gem and test it again with:

   ```sh
   gem install learn-co
   learn whoami
   ```

3. If the `learn` command continues to fail, but RVM is working fine, try
   reinstalling RVM by first using the following command:

   ```sh
   rvm implode
   ```

   Then rerunning the RVM install script:

   ```sh
   curl -sSL https://get.rvm.io | bash -s stable --ruby --auto-dotfiles
   ```

   Once RVM is installed, try reinstalling and testing the `learn-co` gem.

4. If you are still unable to run `learn whoami`, try the following:

   ```sh
   bundle clean --force
   gem install learn-co
   gem install bundler
   ```

   This will clear out any gems that have already been installed. At the moment,
   you will only need the `learn-co` and `bundler` gems, so this reinstalls
   them.

### `learn` Commands Produce `psych` Gem Errors

This error is typically due to issues in the `~/.learn-config` file. 

1.  Run `code ~/.learn-config`. This file should only have three lines in it, 
    similar to the example below:

    ```sh
    ---
    :learn_directory: "/Users/< username >/Flatiron/code"
    :editor: code
    ```

2.  Check for any typos or extra content. Make sure the `:learn_directory` path
    is valid and has your computer's username after `/Users/`. You can confirm this
    name by running `echo $HOME`. 

3.  Save the `.learn-config` file and try running `learn whoami`. 

## Resources

* [Uninstall the Learn IDE][uninstall IDE]
* [Adding SSH Key to GitHub][add ssh]
* [Nokogiri macOS Support][nokogiri support]
* [Visual Studio Code Download][VS Code]

[VS Code]: https://code.visualstudio.com/Download
[nokogiri support]: http://www.nokogiri.org/tutorials/installing_nokogiri.html#mac_os_x
[uninstall IDE]: http://help.learn.co/the-learn-ide/ide-settings/deleting-the-ide
[add ssh]: https://help.github.com/en/enterprise/2.15/user/articles/adding-a-new-ssh-key-to-your-github-account
[Zsh]: https://en.wikipedia.org/wiki/Z_shell
[atom]: https://atom.io/
[octocat gitignore]: https://gist.github.com/octocat/9257657
[Google Chrome]: https://www.google.com/chrome/
[default chrome]: https://support.google.com/chrome/answer/95417?co=GENIE.Platform%3DDesktop&hl=en
[default browser]: https://support.google.com/chrome/answer/95417?co=GENIE.Platform%3DDesktop&hl=en
[your profile]: https://learn.co/account/profile
[GMP]: https://gmplib.org/
[GnuPG]: https://gnupg.org/
[Homebrew]: https://brew.sh/
[RVM]: https://rvm.io/
[wsl]: https://github.com/learn-co-curriculum/wsl-setup
[linux]: https://help.learn.co/en/articles/1489324-setting-up-linux-virtual-box
[optional]: https://github.com/learn-co-curriculum/environment-mac-os-catalina-optional-setup
[Xcode]: https://developer.apple.com/xcode/
[Rails]: https://rubyonrails.org/
[PATH]: https://en.wikipedia.org/wiki/PATH_(variable)
[shell]: https://en.wikipedia.org/wiki/Shell_%28computing%29
