# iSH

iSH is an iOS application to get a Linux environment on any iOS device. This section covers how to install it and configure it so you can use it in your iPad Pro for development.

## Installation

To install iSH on your iPad Pro is as simple as go to the App Store, search for iSH and install it. That's it, open the iSH app to see the motd greeting and the shell prompt waiting for your commands.

## Initial Setup

The setup instructions my change for future versions of iSH, if you are stuck here or want to do something that is not documented, please, refer to the [iSH wiki page](https://github.com/ish-app/ish/wiki).

iSH is made from Alpine, so it uses `apk`, the Alpine Package Manager, to install all the supported applications. It's also an emulation of x86 and syscall translation, meaning, it does not support every software that is supported on Alpine. To know what works and what doesn't work, check this [list](https://github.com/ish-app/ish/wiki/What-works%3F).

To search and install programs use the commands: `apk search <name>` and `apk add <name>`, but first you need to update the Alpine repository list executing:

```bash
apk update
```

You can also delete or update a package using the commands: `apk del <name>` and `apk upgrade`.

Execute the following commands to install some basic packages or programs: 

```bash
apk add vim curl wget less
apk add build-base git openssl make
apk add fortune

# Bash
apk add bash bash-completion

# Python:
apk add python3
python3 -m ensurepip --default-pip

# Ruby:
apk add ruby ruby-dev

```

Installing Bash does not set it as the default shell, continue to the Shell section below to know how to set it as the default shell.

## iSH Settings

Click on the gear at the right bottom corner to open the settings menu. Go to the following options:

-  **Appearance** in this option you can change the theme, for example **Theme** `1337` and **Font** `Menlo`.
- **External Keyboard** if you use a keyboard with your iPad Pro you may want to enable **Hide with external keyboard**. Open the settings menu again pressing `⌘`+`,`.
- **App Icon** to change the app icon to a different one.

## Shell

This section is to install and setup **Zsh** to use it with **Oh-my-Zsh** and the **Powerlevel10k** theme.

To install **Bash** or **Fish** instead of Zsh, and set it as the default shell, execute one of the following pair of commands:

```bash
# Bash
apk add bash bash-doc bash-completion
sed -i.org "s|/bin/ash|$(which bash)|" /etc/passwd

# Fish
apk add fish
sed -i.org "s|/bin/ash|$(which fish)|" /etc/passwd
```

To install  **Zsh** to use it with **Oh-my-Zsh** and the **powerlevel10k** theme, execute the following commands

```bash
apk add zsh
sed -i.org "s|/bin/ash|$(which zsh)|" /etc/passwd

# Required to avoid error: 'vcs_info: function definition file not found'
apk add zsh-vcs

# Install Oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Download powerlevel10k theme
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Open `~/.zshrc` to load the plugins you like. For example:

```bash
plugins=(
	git 
	pip
	sudo 
	golang 
  colorize 
  command-not-found 
  common-aliases 
  emoji 
  emoji-clock 
  themes 
  vagrant 
  aws 
  gcloud 
  npm 
  kubectl 
  helm 
  minikube 
  oc 
  terraform 
  cargo 
  chucknorris
 )
```

Then set the `ZSH_THEME` variable in the same file:

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Close iSH and open it again, to see the **Powerlevel10k configuration wizard**. If you don't see it or if you'd like to re-configure it, execute:

```bash
p10k configure
```

When you finish, close iSH and open it again.

To uninstall or disable Powerlevel10k, select a different theme changing the variable `ZSH_THEME` in the `~/.zshrc` file.

**<u>NOTE</u>**: There is an error when iSH start. I do not have a solution to this error besides it doesn't bother me much.

## SSH

You can install SSH as client to connect to other machines (i.e. a Raspberry Pi),  as a server to connect to your iPad from other computer, or both. 

To install SSH just as a client execute:

```bash
apk add openssh-client
```

Now you are able to `ssh` to any device.

To install SSH as a server and client, execute:

```bash
apk add openssh
```

To configure the SSH service execute the following commands:

```bash
ssh-keygen -A
passwd
echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
/usr/sbin/sshd
```

Locate the iPad IP address in **Settings** > **Wi-Fi**, click on the circled `I` of your Wi-Fi, the IP address is in the section **IPv4 Address**. From other device, execute `ssh root@<IP Address>` and use the password you set.

Next time, just execute the `ssh` service, like so:

```bash
/usr/sbin/sshd
```

To connect from the same iPad, append the `Port` parameter to the file `/etc/ssh/sshd_config`, using a port number greater than `1024`, like so:

```bash
echo 'Port 2200' >> /etc/ssh/sshd_config
```

To connect from other device or from the iPad, use the SSH command like this:

```bash
ssh root@<IP Address> -p 2200
# Or:
ssh localhost -p 2200
```

## Development Environment

I used to have all my programs organized in a Workspace directory, inside I have all the repositories and a Sandbox directory for projects that are not important, with a short live, trainings or that does not required to be in a Git repository.

```bash
mkdir -p ~/Workspace/{Sandbox,johandry}
```

As editor, you can use `vim` or any other editor terminal editor but a better solution is to use an app from the iPad such as [Coda](https://apps.apple.com/us/app/code-editor-by-panic/id500906297), [Koder](https://apps.apple.com/us/app/koder-code-editor/id1447489375#?platform=ipad) or [Texastic](https://apps.apple.com/us/app/textastic-code-editor-9/id1049254261?mt=8). 

Before using any editor, make sure you can see iSH in the Files App. Open **Files App**, click on **Edit** in the top-right corner, and **enable iSH**. Now iSH expose the root directory (`/`) to any App in the iPad through the Files App.

I'm using **Texastic** but the feature to open and create files located in iSH is available on other editors as well.

To create a new file, go to **Create External File...** in the left menu, name the file and select **Choose Location**. Select iSH then choose the directory where to save the new file. Finally click on **Move** in the top-right corner and the file is now in the iSH filesystem, you can check it out going to that directory on iSH. To open an existing file, is similar but instead use the **Open...** option in the left menu.

## Other Programming languages

The following programming languages can be installed on iSH, however for some reason they are not fully functional and it's recommended instead to install them and use them from a Raspberry Pi or other device.

- **Go**: To install Go on iSH execute:

  ```bash
  apk add go
  ```

  Using Go on iSH is not very efficient and it may not work properly.

- **Rust**: To install Rust on iSH execute:

  ```bash
  apk add rust cargo
  ```

  However, Rust on iSH does not build.

- **NodeJS**: To install **NodeJS** on iSH, execute:

  ```bash
  apk add nodejs npm
  ```

  It install without errors but there are some errors trying to install node modules. So, it's not completely ready yet for iSH.

- **Docker**: 

- **Kubernetes**:

- **AWS**:

- **Azure**:

- **Google Cloud**:

- **IBM Cloud**:

