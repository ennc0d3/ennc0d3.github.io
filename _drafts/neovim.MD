# Experimenting neovim (nvim)
Once upon a time there was vi, then came vim(vi iMproved), and now(few years ago actually)
comes neovim (nvim). I remember trying it few years ago but I didn't see a point
to use it when vim8 was out (asynchronous support).

Now that its vacation i am trying to clean few tools and try out few newi
ones, nvim happens to just one of them

## Why
 - 30% lesser code base than ViM ( it doesn't matter to me)
 - better support for plugin (ahh what)
 - built in terminal multiplexer (tmux inside or what, let me find out)
 - deoplete (Yes i wan't so nvim?)


## Installation
I do have an ubuntu(16.04), so i will just care for that,

Add the apt repository and install,(https://github.com/neovim/neovim/wiki/Installing-Neovim|Reference)
```
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt-get update
sudo apt-get install neovim
```

## Get going
```
nvim
:help
:help nvim-from-vim
:checkhealth
```

Followed the installation instruction and created a init.vim file, which inturn
loads my ~/.vimrc. I adjusted few settings in my old vimrc with the if !has('nvim')
switch

After running checkhealth, I did install pynvim and also had to set
the g:python3_host_prog variable

Also I swapped out the Vundle manager with vim-plug (asynch) so i get things fast.

Finally swapped vim to nvim (alias vim = nvim), lets see if i get some hiccups
