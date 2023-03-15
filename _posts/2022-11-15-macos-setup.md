---
title: Professional MacOS dev Setup 2022
---
This document describes how I set up my developer environment on a new MacBook.

The steps below were tested on **macOS Monterey** (12.03), but should work for more recent versions as well.

**Contributing**: If you find any mistakes in the steps described below, or if any of the commands are outdated, do let me know!

## System update

First thing you need to do, on any OS actually, is update the system! For that: **Apple Icon > About This Mac** then **Software Update...**.


## Apple M1 chips
Install the Rosetta2 emulator for the new ARM silicon (M1 chip). Install Rosetta2 using the terminal:
```
softwareupdate --install-rosetta --agree-to-license
```

### Key Points
* In general, binaries built just for x86 architecture will automatically be run in x86 mode
* You can force apps in Rosetta 2 / x86 mode by right-clicking app, click Get Info, check "Open using Rosetta"
* You can force command-line apps by prefixing with `arch -x86_64`, for example `arch -x86_64 go`
* Running a shell in this mode means you don't have to prefix commands: `arch -x86_64 zsh` then `go` or whatever
* Don't just immediately install Homebrew as usual. It should most likely be installed in x86 mode.
* Open ZS in an x86 shell `arch -x86_64 zsh`
* Check which architecture the command support `file $(which git)`
* Check which architecture your term is running `echo $(arch)`
* Update the Python pip that comes in the system. You will need it to install some x86 Python dependencies
  `arch -x86_64 /Library/Developer/CommandLineTools/usr/bin/python3 -m pip install --upgrade pip`

### Troubleshooting
The `pip` command from `pyenv` doesn't support the x86 architecture and you might have problems installing some dependencies. Ex. `cv2`

If you have problems installing dependencies. Use the pip from the system
```
arch -x86_64 /Library/Developer/CommandLineTools/usr/bin/python3 -m pip install DEP
```

## System preferences

If this is a new computer, there are a couple of tweaks I like to make to the System Preferences. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Preferences**:

- Trackpad > Tap to click
- Trackpad > Tracking speed > Fast (all the way to the right)
- Mouse > Tracking speed > Fast (all the way to the right)
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)
- Dock > Automatically hide and show the Dock

### Finder Preferences
- Preferences -> advanced -> show all filename extensions
- View -> Show Path bar
- View -> Show Status bar

## Security

I recommend checking that basic security settings are enabled. You will be happy to have done so if ever your Mac is lost or stolen.

In **Apple Icon > System Preferences**:

- Users & Groups: If you haven't already set a password for your user during the initial set up, you should do so now
- Security & Privacy > General: Require password immediately after sleep or screen saver begins (you can keep a grace period of a couple minutes if you prefer, but I like to know that my computer locks as soon as I close it)
- Security & Privacy > FileVault: Make sure FileVault disk encryption is enabled
- iCloud: If you haven't already done so during set up, enable Find My Mac



## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for macOS is [Homebrew](http://brew.sh/).

### Install

An important dependency before Homebrew can work is the **Command Line Developer Tools** for **Xcode**. These include compilers that will allow you to build things from source. You can install them directly from the terminal with:

```
xcode-select --install
```

Once that is done, we can install Homebrew by copy-pasting the installation command from the [Homebrew homepage](http://brew.sh/) inside the terminal:

```
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Follow the steps on the screen. You will be prompted for your user password so Homebrew can set up the appropriate permissions.

Once installation is complete, you can run the following command to make sure everything works:

```
brew doctor
```

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

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

### Homebrew Services

A nice extension to Homebrew is [Homebrew Services](https://github.com/Homebrew/homebrew-services). It will automatically launch things like databases when your computer starts, so you don't have to do it manually every time.

Homebrew Services will automatically install itself the first time you run it, so there is nothing special to do.

After installing a service (for example a database), it should automatically add itself to Homebrew Services. If not, you can add it manually with:

```
brew services <formula>
```

Start a service with:

```
brew services start <formula>
```

At anytime you can view which services are running with:

```
brew services list
```

## iTerm2

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one.
```
brew install --cask iterm2
```

You can now launch iTerm, through the **Launchpad** for instance.

### Natural text editing
To improve the experience in the terminal enable the Natural text editing option

**Preferences -> profiles -> keys -> Key Mappings -> Add Natural text editing**

## Warp Terminal
I started using [Warp terminal](https://www.warp.dev/) instead of Iterm2. I think worth check it out.

```
brew install --cask warp
```

### Install oh-my-zsh

ZSH is already preinstalled in the latest versions of macOS. I also install https://ohmyz.sh/ as it allows for more configuration and is required in some cases.

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install the Oh My Zsh plugins below
```
brew install zsh-autosuggestions
brew install zsh-syntax-highlighting
```

To activate the plugins, add the following at the end of your .zshrc:
```
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Add the `source ~/.bash_profile` to .zshrc, this way you will leverage the configuration from that file

Change the ZSH theme. Replace the `ZSH_THEME` with `ZSH_THEME="gnzh"`.


You will also need to force reload of your .zshrc:

```
source ~/.zshrc
```

## Stats bar
Display macOS system monitor in your menu bar
```
brew install stats
```

## Create ssh key

Execute the command below to begin the key creation. Press enter all the way to the end
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Add the new SSH key to the local SSH agent.
```
ssh-add -K /Users/YOUR_USER/.ssh/id_rsa
```

Get the generated public key
```
cat /Users/alexsouza/.ssh/id_rsa.pub
```

## Show all hidden files
Use the command line to show all hidden files as the files you are searching for are going to be hidden by default.

```
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

## Git

macOS comes with a pre-installed version of [Git](http://git-scm.com/), but we'll install our own through Homebrew to allow easy upgrades and not interfere with the system version. To do so, simply run:

```
brew install git
```

When done, to test that it installed fine you can run:

```
which git
```

The output should be `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig) file to your home directory:

```
cd ~
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig
```

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

They will get added to your `.gitconfig` file.

On a Mac, it is important to remember to add `.DS_Store` (a hidden macOS system file that's put in folders) to your project `.gitignore` files. You also set up a global `.gitignore` file, located for instance in your home directory (but you'll want to make sure any collaborators also do it):

```
cd ~
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitignore
git config --global core.excludesfile ~/.gitignore
```

## Visual Studio Code

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but if you're just getting started and looking for something simple that works, [Visual Studio Code](https://code.visualstudio.com/) is a pretty good option.

Go ahead and [download](https://code.visualstudio.com/Download) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

If you remember only one keyboard shortcut in VS Code, it should be **Cmd+Shift+P**. This opens the **Command Palette**, from which you can run pretty much anything.

Let's open the command palette now, and search for `Shell Command: Install 'code' command in PATH`. Hit enter when it shows up. This will install the command-line tool `code` to quickly open VS Code from the terminal. When in a projects directory, you'll be able to run:

```
cd myproject/
code .
```

## Vim

Although VS Code will be our main editor, it is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you only use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

Using Vim's built-in package support, install these settings by running:

```
mkdir -p ~/.vim/pack/tpope/start
cd ~/.vim/pack/tpope/start
git clone https://tpope.io/vim/sensible.git
```

With that, Vim will look a lot better next time you open it!

## Python

macOS, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version using [pyenv](https://github.com/yyuu/pyenv). This will also allow us to manage multiple versions of Python (ex: 2.7 and 3) should we need to.

Install `pyenv` via Homebrew by running:

```
brew install pyenv
```

When finished, you should see instructions to add something to your profile. Open your `.bash_profile` in the home directory (you can use `code ~/.bash_profile`), and add the following line:

```bash
if command -v pyenv 1>/dev/null 2>&1; then eval "$(pyenv init -)"; fi
```

Save the file and reload it with:

```
source ~/.bash_profile
```

Before installing a new Python version, the [pyenv wiki](https://github.com/pyenv/pyenv/wiki) recommends having a few dependencies available:

```
brew install openssl readline xz
```

We can now list all available Python versions by running:

```
pyenv install --list
```

Look for the latest 3.x version (or 2.7.x), and install it (replace the `.x.x` with actual numbers):

```
pyenv install 3.x.x
```

List the Python versions you have locally with:

```
pyenv versions
```

The star (`*`) should indicate we are still using the `system` version, which is the default.

```
pyenv global 3.x.x
```

You can switch your current terminal to another Python version with:

```
pyenv shell 3.x.x
```

You should now see that version when running:

```
python --version
```

In a project directory, you can use:

```
pyenv local 3.x.x
```

This will save that project's Python version to a `.python-version` file. Next time you enter the project's directory from a terminal, `pyenv` will automatically load that version for you.

For more information, see the [pyenv commands](https://github.com/yyuu/pyenv/blob/master/COMMANDS.md) documentation.

### pip

[pip](https://pip.pypa.io) was also installed by `pyenv`. It is the package manager for Python.

Here are a couple Pip commands to get you started. To install a Python package:

```
pip install <package>
```

To upgrade a package:

```
pip install --upgrade <package>
```

To see what's installed:

```
pip freeze
```

To uninstall a package:

```
pip uninstall <package>
```

### virtualenv

[virtualenv](https://virtualenv.pypa.io) is a tool that creates an isolated Python environment for each of your projects.

For a particular project, instead of installing required packages globally, it is best to install them in an isolated folder, that will be managed by `virtualenv`. The advantage is that different projects might require different versions of packages, and it would be hard to manage that if you install packages globally.

Instead of installing and using `virtualenv` directly, we'll use the dedicated `pyenv` plugin [pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv) which will make things a bit easier for us. Install it via Homebrew:

```
brew install pyenv-virtualenv
```

After installation, add the following line to your `.bash_profile`:

```bash
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

And reload it with:

```
source ~/.bash_profile
```

Now, let's say you have a project called `myproject`. You can set up a virtualenv for that project and the Python version it uses (replace `3.x.x` with the version you want):

```
pyenv virtualenv 3.x.x myproject
```

See the list of virtualenvs you created with:

```
pyenv virtualenvs
```

To use your project's virtualenv, you need to **activate** it first (in every terminal where you are working on your project):

```
pyenv activate myproject
```

If you run `pyenv virtualenvs` again, you should see a star (`*`) next to the active virtualenv.

Now when you install something:

```
pip install <package>
```

It will get installed in that virtualenv's folder, and not conflict with other projects.

You can also set your project's `.python-version` to point to a virtualenv you created:

```
pyenv local myproject
```

Next time you enter that project's directory, `pyenv` will automatically load the virtualenv for you.

### Anaconda and Miniconda

The Anaconda/Miniconda distributions of Python come with many useful tools for scientific computing.

You can install them using `pyenv`, for example (replace `x.x.x` with an actual version number):

```
pyenv install miniconda3-x.x.x
```

After loading an Anaconda or Miniconda Python distribution into your shell, you can create [conda](https://docs.conda.io/) environments (which are similar to virtualenvs):

```
pyenv shell miniconda3-x.x.x
conda create --name  mycondaproject
conda activate mycondaproject
```

Install packages, for example the [Jupyter Notebook](https://jupyter.org/), using:

```
conda install jupyter
```

You should now be able to run the notebook:

```
jupyter notebook
```

Deactivate the environment, and return to the default Python version with:

```
conda deactivate
pyenv shell --unset
```

### Known issue: `gettext` not found by `git` after installing Anaconda/Miniconda

If you installed an Anaconda/Miniconda distribution, you may start seeing an error message when using certain `git` commands, similar to this one:

```
pyenv: gettext.sh: command not found

The `gettext.sh' command exists in these Python versions:
  miniconda3-latest
```

If that is the case, you can use the following [workaround](https://github.com/pyenv/pyenv/issues/688#issuecomment-428675578):

```
brew install gettext
```

Then add this line to your `.bash_profile`:

```bash
# Workaround for: https://github.com/pyenv/pyenv/issues/688#issuecomment-428675578
export PATH="/usr/local/opt/gettext/bin:$PATH"
```

## pipx
pipx is a tool to help you install and run end-user applications written in Python.

```
brew install pipx
pipx ensurepath
```

## AWS CLI

Download the pkg installer using the curl command.
```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
```
Run the standard macOS installer program, specifying the downloaded .pkg file as the source.
```
sudo installer -pkg ./AWSCLIV2.pkg -target /
```

If you don't have root permission check [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) how to install to the current user without `sudo`


## Node.js

The recommended way to install [Node.js](http://nodejs.org/) is to use [nvm](https://github.com/creationix/nvm) (Node Version Manager) which allows you to manage multiple versions of Node.js on the same machine.

Install `nvm` by copy-pasting the [install script command](https://github.com/creationix/nvm#install--update-script) into your terminal.

Once that is done, open a new terminal and verify that it was installed correctly by running:

```
command -v nvm
```

View the all available stable versions of Node with:

```
nvm ls-remote --lts
```

Install the latest stable version with:

```
nvm install node
```

It will also set the first version installed as your default version. You can install another specific version, for example Node 10, with:

```
nvm install 10
```

And switch between versions by using:

```
nvm use 10
nvm use default
```

See which versions you have install with:

```
nvm ls
```

Change the default version with:

```
nvm alias default 10
```

In a project's directory you can create a `.nvmrc` file containing the Node.js version the project uses, for example:

```
echo "10" > .nvmrc
```

Next time you enter the project's directory from a terminal, you can load the correct version of Node.js by running:

```
nvm use
```

### npm

Installing Node also installs the [npm](https://npmjs.org/) package manager.

To install a package:

```
npm install <package> # Install locally
npm install -g <package> # Install globally
```

To install a package and save it in your project's `package.json` file:

```
npm install --save <package>
```

To see what's installed:

```
npm list --depth 1 # Local packages
npm list -g --depth 1 # Global packages
```

To find outdated packages (locally or globally):

```
npm outdated [-g]
```

To upgrade all or a particular package:

```
npm update [<package>]
```

To uninstall a package:

```
npm uninstall --save <package>
```

### Yarn

Is a alternative package manage to npm

```
npm install --global yarn
```

## Java

The recommended way to install Java is to use [SDKman](https://sdkman.io/) (Software development kit Management) which allows you to manage multiple versions of Java on the same machine and

### Install

```
curl -s "https://get.sdkman.io" | bash
```

### Usage

The following command will show you which versions of Java are available to install:

```
sdk list java
```

You can find the latest version in that list and install it with (replace `.x.x` with actual version numbers):

```
sdk install java 19.x.x
```

Select the version you want to use

```
sdk use java 11.x.x
```

### Java version per Project
Want to switch to a specific JDK or SDK every time you visit a project? This can be achieved through an `.sdkmanrc` file in the base directory of your project. This file can be generated automatically by issuing the following command
```
sdk env init
```

To switch to the configuration present in your .sdkmanrc file, simply issue the following command:

```
sdk env
```


### Managing Java tools
Check all Java softwares available [here](https://sdkman.io/sdks)

Install Maven

```
sdk install maven
```

Install VisualVM

```
sdk install visualvm
```

## Postman
Postman is an API platform for building and using APIs. It is a very useful tool to work with different APIs(REST, Graphql and gRPC)
```
brew install --cask postman
```

## AltTab
AltTab brings the power of Windows’s “alt-tab” window switcher to macOS.
```
brew install --cask alt-tab
```

After installed, use option + tab to navigate through the opened windows

## Zoom
Video communication and virtual meeting platform

```
brew install --cask zoom
```

## Slack
Team communication and collaboration software

```
brew install --cask slack
```

## Docker

[Docker](https://www.docker.com/)  is a set of platform-as-a-service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.

### Install

Download the version of docker for osx you want, check [here](https://docs.docker.com/install/overview/)

Create an account [here](https://hub.docker.com/)

Then you can download [here](https://hub.docker.com/?overlay=onboarding)

Follow all the steps and congrats!You should have downloaded docker


## Kubernetes
We will quickly and easily get a Kubernetes cluster up and running by running a `kind` Kubernetes cluster. [Kind](https://kind.sigs.k8s.io/) is a Kubernetes cluster that leverages Docker to run a cluster.

```
brew install kind

// Create a new cluster
kind create cluster --name myloca
```

Deploy a sample workload(Nginx)
```
kubectl apply -f https://k8s.io/examples/application/deployment.yaml

// See the pods created
kubectl get po
```


## PostgreSQL

[PostgreSQL](https://www.postgresql.org/) is a popular relational database, and Heroku has first-class support for it.

Install PostgreSQL using Homebrew:

```
brew install postgresql
```

It will automatically add itself to Homebrew Services. Start it with:

```
brew services start postgresql
```

If you reboot your machine, PostgreSQL will be restarted at login.

### GUI

You can interact with your SQL database by running `psql` in the terminal.

If you prefer a GUI (Graphical User Interface), [Postico](https://eggerapps.at/postico/) has a simple free version that let's you explore tables and run SQL queries.

## Redis

[Redis](http://redis.io/) is a fast, in-memory, key-value store, that uses the disk for persistence. It complements nicely a database such as PostgreSQL. There are a lot of [interesting things](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) that you can do with it. For example, it's often used for session management or caching by web apps, but it has many other uses.

To install Redis, use Homebrew:

```
brew install redis
```

Start it through Homebrew Services with:

```
brew services start redis
```

I'll let you refer to Redis' [documentation](http://redis.io/documentation) or other tutorials for more information.

## Elasticsearch

[Elasticsearch](https://www.elastic.co/products/elasticsearch) is a distributed search and analytics engine. It uses an HTTP REST API, making it easy to work with from any programming language.

You can use elasticsearch for things such as real-time search results, autocomplete, recommendations, machine learning, and more.

### Install

Elasticsearch runs on Java, so make sure you have installed Java already.

Next, install Elasticsearch with:

```bash
brew install elasticsearch
```

### Usage

Start the Elasticsearch server with:

```bash
brew services start elasticsearch
```

Test that the server is working correctly by running:

```bash
curl -XGET 'http://localhost:9200/'
```

(You may need to wait a little bit for it to boot up if you just started the service.)

Elasticsearch's [documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html) is more of a reference. To get started, you can also take a look at [Elasticsearch: The Definitive Guide](https://www.elastic.co/guide/en/elasticsearch/guide/master/index.html).

### GUI

You can interact with the Elasticsearch server using `curl`, or anything that can send an HTTP request.

However, if you prefer a graphical interface, you can take a look at [Dejavu](https://opensource.appbase.io/dejavu/). You can easily install it via the [Dejavu Chrome Extension](https://chrome.google.com/webstore/detail/dejavu-elasticsearch-web/jopjeaiilkcibeohjdmejhoifenbnmlh).




