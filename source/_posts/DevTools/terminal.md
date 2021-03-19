
---
title: Custom Terminal
date: 2021-03-22 16:27:47
tags: tools, 
description: " zsh, shell, antigen,  oh-my-zsh"
---


1. install Antigen
2. confing ~/.zshrc
	```shell
	    # Source Antigen
	    source /usr/local/share/antigen/antigen.zsh
	 
	    # Load Antigen configurations
	    antigen init ~/.antigenrc
	```
3. config ~/.antigenrc
	```shell
	#Load oh-my-zsh plugin
	antigen use oh-my-zsh
	
	# Load bundles from the default repo (oh-my-zsh)
	antigen bundle git
	antigen bundle command-not-found
	antigen bundle docker
	antigen bundle brew
	antigen bundle docker-compose
	antigen bundle gem
	
	# Load bundles from external repos
	antigen bundle zsh-users/zsh-completions
	antigen bundle zsh-users/zsh-autosuggestions
	antigen bundle zsh-users/zsh-syntax-highlighting
	antigen bundle zsh-users/zsh-apple-touchbar
	
	# Select theme
	antigen theme denysdovhan/spaceship-prompt
	
	# Tell Antigen that you're done
	antigen apply
	```
4. *source .zshrc* and reopen terminal
