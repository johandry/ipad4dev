# Setup

This section covers all the instructions to setup the environment on iSH or the Raspberry Pi to make them ready to start coding in multiple programming languages.

- [Setup](#setup)
  - [Shell](#shell)
  - [Visual Studio](#visual-studio)
  - [Go](#go)
  - [Python](#python)
  - [Rust](#rust)

## Shell

This section is to install and setup **Oh-my-Zsh** and the **Powerlevel10k** theme having **Zsh** as the default shell.

After installing Zsh, download and install Oh-my-zsh, then download the Powerlevel10k theme, executing the following commands:

```bash
# Install Zsh on iSH
apk add zsh
# Install Zsh on the Rasberry Pi

sed -i.org "s|/bin/ash|$(which zsh)|" /etc/passwd

# Install Oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Download powerlevel10k theme
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

vim ~/.zshrc
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
  docker 
  docker-compose 
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

When you finish, close the terminal or iSH and open it again. 

To uninstall or disable Powerlevel10k, select a different theme changing the variable `ZSH_THEME` in the `~/.zshrc` file.

## Development Environment



## Go



## Python



## Rust



## Node



## Docker



## Kubernetes



## Clouds