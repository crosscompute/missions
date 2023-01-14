# Vision
Train the next generation of engineers who are going to build the future.

# Mission
Document how to set up a development environment in Fedora 34.

# Owner
Roy Hyunjin Han

# Roles
Mentor: Roy

# Context
If you want to build software, you are going to have to know how to set up your workspace.

# Timeframe
20211023-2100 - 20211030-2300: 1 week

# Budget
N/A

# Objectives
1. Set up your command line environment.
2. Set up ssh keys for version control.

# Habits
N/A

# Log

## Saturday 20211023-2145 - 20211023-2200: 15 minutes

We are setting up a development environment from scratch starting from Fedora 34.

```
ssh-keygen -t ed25519 -C rhh@violenthellfire
```

	[Done] Create new ssh keys
	[Done] Add them to github
	[Done] Set up ssh keys

	sudo vim /etc/sudoers
		# %wheel        ALL=(ALL)       ALL
		%wheel  ALL=(ALL)       NOPASSWD: ALL

	[Done] Make it so we do not have to specify password for sudo

	cd ~/Documents
	git clone https://github.com/invisibleroads/scripts
	cd scripts
    # Install configuration files
	bash setup.sh
    # Configure a virtual environment
	bash setup-environment.sh
    # Install dependencies for vim plugins
	bash setup-packages.sh

    # Reload bashrc
    source ~/.bashrc

    [Done] Set up development environment configuration

## Saturday 20211023-2215 - 20211023-2230: 15 minutes

    [Done] Create repositories
        https://github.com/crosscompute/missions

# Schedule

    Clean up plugins in vimrc
    Review shortcuts for each plugin that we want to learn
    Explain how to use tmux
        Split horizontally / vertically

# Tasks

    Show how to install JupyterLab
    Add link to mission template

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
