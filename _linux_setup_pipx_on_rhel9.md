
# Linux Users - setup pipx on Red Hat 9

## NOTE: pipx uses pypi.org as the source for installing Python packages

## Install pipx on RHEL9

```
$ sudo dnf install python3.12-pip

$ python3.12 -m pip install --user pipx

$ python3.12 -m pipx ensurepath

$ pipx environment

```

# Using pipx

## EXAMPLE 1: 

### install cowsay by pipx 

```
$ pipx install cowsay

$ cowsay -t 'hello world!'
  ____________
| hello world! |
  ============
            \
             \
               ^__^
               (oo)\_______
               (__)\       )\/\
                   ||----w |
                   ||     ||


```

## EXAMPLE 2: 

### Install the latest version of ansible-core

```
$ pipx install ansible-core

```

### Uninstall ansible-core

```
$ pipx uninstall ansible-core

```

### Install specific version of ansible-core

```
$ pipx install ansible-core==2.16.4

```

### Update to specific version of ansible-core

```
$ pipx install ansible-core==2.16.5 --force

```

### Upgrade to the lastest available version of ansible-core

```
$ pipx upgrade ansible-core

```

