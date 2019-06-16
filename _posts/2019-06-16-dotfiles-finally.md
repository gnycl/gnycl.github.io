---
layout: post
title: Dotfiles, finally
---

I've tried different methods of keeping track of my dotfile configurations. I have scrap repos and emails to myself going back more than a decade, but never found a good solution, until recently. This isn't my own solution, but something I came across [on HN](https://news.ycombinator.com/item?id=11071754).

The big idea is you have an `~/.dotfiles` directory tracked with git. Then, you have an alias to always use this directory, no matter where you are in the filestystem.Then finally, set git to not show untracked files when committing.

---

```sh
git init --bare $HOME/.dotfiles
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
config config status.showUntrackedFiles no
```

Then, to use it you can do something like this:

```sh
config status
config add .vimrc
config commit -m "Add vimrc"
config push
```

Now, you can have your dotfiles on a new machine pretty simply by first cloning the repo's working directory into a temporary directory:

```
git clone --separate-git-dir=$HOME/.dotfiles /path/to/repo $HOME/dotfiles-tmp
rm -r ~/dotfiles-tmp/
```

Set your alias:

`alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'`

Finally, clone into the ~ directory of your new machine:

`git clone --separate-git-dir=~/.dotfiles /path/to/repo ~`

One of the nice features of this method is you can manage machine-specific dotfiles with branches. For example, if you wanted to track work-computer-specific configuration: `git co -b work_config`

This has been a more maintainable method of managing and tracking my configuration than anything I've tried in the past. I mostly wrote this as reference for myself, but maybe someone else also finds it useful.
