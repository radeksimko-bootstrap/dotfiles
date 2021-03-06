# Dotfiles

My Unix dotfiles.

### OS X - Before installation
You need to have installed [XCode](https://developer.apple.com/downloads/index.action?=xcode) or, at the very minimum, the [XCode Command Line Tools](https://developer.apple.com/downloads/index.action?=command%20line%20tools), which are available as a _much smaller_ download thank XCode.

## How to use it?
Fork the boostrap project, add your own dotfiles
into [appropriate dirs](#all-steps-in-detail) and continue onto next paragraph.

## How to install it?
With Git:
```sh
git clone '<fork-repo-url>' ~/.dotfiles
~/.dotfiles/bin/dotfiles [-i]
```

without Git (it will try to install Git which **implies superuser privileges**):
```sh
bash -c "$(curl -fsSL https://raw.github.com/<fork-user-name>/<fork-repo-name>/<branch-name>/bin/dotfiles)" dotfiles -r https://github.com/<fork-user-name>/<fork-repo-name>[@<branch-name>] [-i]
```

If (optional) `-i` is passed, the [init step](#the-init-step) will be executed.
It is optional because you might not have privileges to install anything.

### TL;DR What does the "dotfiles" do?

When [dotfiles](bin/dotfiles) runs, it does a few things:

1. Files in `init` are executed (if `-i` option is provided)
2. Files in `copy` are copied into `~/`
3. Files in `link` are linked into `~/`
4. Files in `source` are _sourced_
5. `conf/first_time_reminder.sh` is executed after first-time installation

#### Notes

* The `backups` folder only gets created when necessary. Any files in `~/` that would have been overwritten by `copy` or `link` get backed up there.
* Files in `source` get sourced whenever a new shell is opened (in alphanumeric order).
* Files in `conf` just sit there. If a config file doesn't _need_ to go in `~/`, put it in there.

### All steps in detail

#### The "init" step
A whole bunch of things will be installed _only_ if `-i` option is provided and _only_ if they aren't already installed.

#### The ~/ "copy" step
Any file in the `copy` subdirectory will be copied into `~/`. Any file that _needs_ to be modified with personal information (like [.gitconfig](copy/.gitconfig) which contains an email address and private key) should be _copied_ into `~/`. Because the file you'll be editing is no longer in `~/.dotfiles`, it's less likely to be accidentally committed into your public dotfiles repo.

#### The ~/ "link" step
Any file in the `link` subdirectory gets symbolically linked with `ln -s` into `~/`. Edit these, and you change the file in the repo. Don't link files containing sensitive data, or you might accidentally commit that data!

#### The "source" step
To keep things easy, the `~/.bashrc` and `~/.bash_profile` files are extremely simple, and should never need to be modified. Instead, add your aliases, functions, settings, etc into one of the files in the `source` subdirectory, or add a new file. They're all automatically sourced when a new shell is opened.

### Helpers
There are some helper functions you may find useful while installing stuff:

```sh
recipes=(
  ack
  hub
)
INSTALL_BREW_PACKAGES "${recipes[*]}"
```
```sh
taps=(
  caskroom/cask
  caskroom/versions
)
TAP_BREW_REPOS "${taps[*]}"
```
```sh
apps=`cat <<LIST
  twitter/id409789998 Twitter.app
  xcode/id497799835 Xcode.app
LIST`
INSTALL_APPSTORE_APPS "$apps"
```
```sh
packages=(
  vmware-fusion
  the-unarchiver
)
INSTALL_CASK_PACKAGES "${packages[*]}"
```

## Inspiration
- https://github.com/cowboy/dotfiles (the `dotfiles` + dirs structure/philosophy)

