

### What is this?
This is the ansible working environment with a full working test kitchen (for local integration).  You can spin up VMs locally using the same code we're using to build the 'prod' environment.

### Setup

You'll need a few things to make this work.  The easiest way to get started is with Homebrew and Cask:

```
\# Install Homebrew: 
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

\# Install cask:
brew install caskroom/cask/brew-cask

\# Install vagrant, virtualbox, and chefdk (chefdk has a clean and sane ruby env)
brew cask update
brew cask install vagrant virtualbox chefdk

\# Install ansible (for environment control)
brew install ansible

\# Put this in your ~/.bash_profie
export PATH="/opt/chefdk/embedded/bin:$PATH"

\# Add bash completion and git prompt:
brew install bash-git-prompt bash-completion 

\# If you want fancy-pants git prompts, add this too:
# Autocomplete
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi

# Fancy-pants git stuff
function git-branch-name {
	git symbolic-ref HEAD 2>/dev/null | cut -d"/" -f 3
}

function git-branch-prompt {
	local branch=`git-branch-name`
	if [ $branch ]; then printf " [%s]" $branch; fi
}

export PS1="\u@\h \[\033[0;36m\]\W\[\033[0m\]\[\033[0;32m\]\$(git-branch-prompt)\[\033[0m\] \$ "

\# Run this to apply your ~/.bash_profile.
source ~/.bash_profile

\# Verify things look right
which gem   (->)   /opt/chefdk/embedded/bin/gem

\# Install gems
gem install test-kitchen kitchen-vagrant kitchen-ansible

```

### Let's get this party started!

If everything went well, once you clone this repo, cd into it at the CLI and run:
```kitchen list```

Depending on the state of the repo and whatnot, you should get something like this:
```
➜  doomdemo git:(master) kitchen list
Instance           Driver   Provisioner      Verifier  Transport  Last Action
host1-ubuntu-1404  Vagrant  AnsiblePlaybook  Busser    Ssh        <Not Created>
host2-ubuntu-1404  Vagrant  AnsiblePlaybook  Busser    Ssh        <Not Created>
```


If that's the case, then do this:
```kitchen converge```

### Links
https://insights.ubuntu.com/2015/05/06/live-migration-in-lxd/

Ansible Best Practice Docs -- These are what this repo is modeled after:
http://docs.ansible.com/playbooks_best_practices.html

Test kitchen using Ansible:
https://github.com/neillturner/kitchen-ansible

Provisioner options:
https://github.com/neillturner/kitchen-ansible/blob/master/provisioner_options.md

### Examples!
https://github.com/ansible/ansible-examples/blob/master/lamp_haproxy/roles/common/tasks/main.yml

https://gist.github.com/marktheunissen/2979474
