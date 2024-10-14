---
date: 2024-10-14T15:28:01.421Z
publishDate: 2024-10-14T15:28:01.421Z
title: Compiling Emacs 30 on macos
summary: New mac, new emacs install. I had to remind myself what I did last time, so I'm documenting it here in the hopes that next time I won't spend so long on this.
category:
  - macos
  - emacs
---

To get Emacs v30, I'm using the [emacs plus](https://github.com/d12frosted/homebrew-emacs-plus) formula for homebrew. It is installed like this:

    brew tap d12frosted/emacs-plus
    brew install emacs-plus@30 --with-imagemagick --with-native-comp

This does a compile from source, so takes a while.

Finally, to create an alias from the app in the homebrew install location to the Applications directory, the brew script says to run this command:

    osascript -e 'tell application "Finder" to make alias file to posix file "/opt/homebrew/opt/emacs-plus@30/Emacs.app" at POSIX files "/Applications" with properties {name:"Emacs.app"}'

After that, emacs can be launched from the command line or the UI.

[My emacs config](https://github.com/gilesp/literate_emacs) is in git, so I just need to pull that to get everything set up as I like it.

Although, I do need to install my preferred fonts as well:

    brew install --cask font-fira-code font-roboto

And within emacs, I need to run `M-x nerd-icons-install-fonts` to get the modeline icons (I could probably install these via brew as well)


