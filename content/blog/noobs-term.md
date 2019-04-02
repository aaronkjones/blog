+++
date = "2018-11-07"
title = "Noobs Term"
description = "Introduction to Noobs Term"
images = []
series = ["terminal", "zsh", "tmux", "vim"]
+++

## Introduction

[Noobs Term](https://noobs-term.com) is a terminal bundle that combines a few powerful programs with "sensible" configurations and a beautiful theme.

I made Noobs Term to give people a foundation from which they can begin working with Zsh and Tmux without being overwhelmed with many customizations and options up front. When I first started using Zsh and Tmux, I found many configurations that offered some customization that I liked and some I either didn't like or didn't understand.

Projects like [tmux-sensible](https://github.com/tmux-plugins/tmux-sensible) and [vim-sensible](https://github.com/tpope/vim-sensible) aim to establish default settings that are universally accepted as "sensible". [Noobs Term](noobs-term.com) combines these programs and configurations with [spaceship-prompt](https://github.com/denysdovhan/spaceship-prompt), a very featureful, customizable Zsh prompt; and Nord, a beautiful arctic, bluish theme.

The goal of Noobs Term is to have a well-documented, well-supported, and "sensible", all-in-one package that works for not only "noobs", but advanced users alike.

## Included

| Feature                                                             | Description                                                                                   |
| ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [zsh](https://en.wikipedia.org/wiki/Z_shell)                        | a popular shell with features like completion, path correction, spelling correction, and more |
| [tmux](https://github.com/tmux/tmux)                                | terminal multiplexer allows you to manage multiple terminal sessions from a single window       |
| [neovim](https://neovim.io/)                                        | a project that seeks to aggressively refractor Vim                                             |
| [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)              | a framework for managing your zsh configuration                                               |
| [vim-sensible](https://github.com/tpope/vim-sensible)               | vim settings everyone can agree on                                                            |
| [tmux-sensible](https://github.com/tmux-plugins/tmux-sensible)      | tmux settings everyone can agree on                                                           |
| [nord-tmux](https://github.com/arcticicestudio/nord-tmux)           | An arctic, north-bluish clean and elegant tmux color theme                                    |
| [nord-vim](https://github.com/arcticicestudio/nord-vim)             | An arctic, north-bluish clean and elegant Vim color theme                                     |
| [spaceship-prompt](https://github.com/denysdovhan/spaceship-prompt) | A zsh prompt for Astronauts                                                                   |

## Features

Tmux provides many benefits including, persistence (session resumption across disconnects or reboots), terminal multiplexing (window splitting, tabbing), and much more. If you've ever been working on a remote system over SSH and had your connection drop or computer restart, you know the frustration of having to start over. With Tmux, once you reconnect you can reattach to your session and resume working.

![resume after close](https://i.imgur.com/YKtoo2l.gif)
Resuming session after close

![resume after disconnect](https://i.imgur.com/OptqyRK.gif)
Resuming session after disconnect

For a full list of features see [showcase](https://noobs-term.com/#/?id=showcase).

## Going beyond the basics

Just like with "vanilla" Tmux and Zsh, you can modify to your hearts desire. Noobs Term places all configurations in ~/.dotfiles and symbolically links them to their default locations (e.g. `~/.dotfiles/.zshrc --> ~/.zshrc`).

Noobs Term currently works on Debian, Ubuntu, Raspbian, Arch Linux, Mac OSX, and Windows. For documentation and instructions on how to install it, visit [Noobs Term](https://noobs-term.com).