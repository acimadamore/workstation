# Workstation

This Ansible playbook helps me to quickly set up my workstation without too much hassle.

![](https://media.giphy.com/media/XreQmk7ETCak0/giphy.gif)

All software installed and configurations used reflect my work environment. You're more than welcome to use this playbook as you see fit, i'll be grateful for any issue reported or suggestion made.

## Overview

This playbook run the following tasks.

- Install applications/packages using APT from official repositories. Currently:
  - bat
  - chromium-browser
  - exuberant-ctags
  - git
  - htop
  - httpie
  - jq
  - lm-sensors
  - mtr
  - neofetch
  - network-manager-openvpn
  - nmap
  - python3-dev
  - python3-pip
  - python3-venv
  - ranger
  - screen
  - silversearcher-ag
  - thefuck
  - tmux
  - tree
  - unzip
  - vim-gtk
  - vagrant
  - virtualbox

- Install specified snaps.

- Install [Visual Studio Code](https://code.visualstudio.com/).

- Install and configure [fzf](https://github.com/junegunn/fzf).

- Install and configure [direnv](https://direnv.net/).

- Install and configure [pyenv](https://github.com/pyenv/pyenv). Install specified Python versions and sets one as default.

- Install and configure [rbenv](https://github.com/rbenv/rbenv). Install specified Ruby versions and sets one as default.

- Install and configure [fnm](https://github.com/Schniz/fnm). Install specified Node.js versions and sets one as default.

- Install [Docker](https://www.docker.com/) and [docker-compose](https://github.com/docker/compose).

- Install [my dotfiles](https://github.com/acimadamore/dotfiles).

Many configurations such as installed applications, language interpreters' versions and dotfiles repository can be easily changed. Check out [Change configurations](#change-configurations).

## Use

A Python 3 interpreter is required to run this playbook. Most Linux distributions come with some version out of the box.

1. Download or clone this repository to your workstation.
2. **(optional)** Create and activate a virtual environment. On the repository's directory: `python3 -m venv venv && source ./venv/bin/activate`.
3. Upgrade Pip: `pip3 install --upgrade pip`
4. Install Ansible and other dependencies: `pip3 install -r requirements.txt`
5. Install required Ansible roles & collections: `ansible-galaxy install -r requirements.yml`
6. Run the playbook: `ansible-playbook workstation.yml --ask-become-pass`

**NOTES**
- The playbook uses privilege escalation to execute some tasks; the user that runs it needs to have the correct permissions (be a sudoer for example).
- If you are using Ubuntu you will need to install the packages `python3-pip` and `python3-venv` before step 2.

## Customization

### Skip or run specific tasks

If you need to, you can skip parts of this playbook or run only a subset of the tasks. Run this playbook specifying a set of tags using the `--tags` flag:

```
ansible-playbook workstation.yml --ask-become-pass --tags packages,docker,dotfiles
```

The tags available are: `packages`, `snaps`, `code` / `visual-code`, `fzf`, `direnv`, `interpreters` ( `pyenv` / `python`, `rbenv` / `ruby`, `fnm` / `node` ), `docker`, `dotfiles`.

You can avoid/skip certain tasks using the inverse flag `--skip-tags`.

### Provision a remote machine

You can provision a remote workstation using this playbook; the remote machine should be configured so you can access to it through [SSH](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-20-04/).

Edit the inventory file _"inventory/local"_ in this repository:
```
# before
[workstation]
localhost ansible_connection=local

# after
[workstation]
<remote-workstation-ip/hostname> ansible_user=<remote-workstation-user>
```

Then run the playbook:
```
ansible-playbook workstation.yml
```

### Change configurations

You can change the configuration on _"vars/main.yml"_ to customize the installed packages, the versions of ruby to install and to use by default, etc:

```yaml
apt_packages:
  - htop
  - screen
  - steam
  - unzip

snap_packages:
  - spotify

ruby_versions:
  - 2.7.6
  - 3.2.0-dev
ruby_default_version: 2.7.6

dotfiles_repository: https://github.com/johndoe/dotfiles
dotfiles_repository_path: ~/dotfiles
```

## License

MIT

## Author

[Andrés Cimadamore](https://github.com/acimadamore)
