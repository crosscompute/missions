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
20211023-2100 - 20211023-2300: 2 hours

# Budget
Free

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

# Schedule

    + Create repositories
        missions
    Clone repositories
        crosscompute
        crosscompute-docs
    Work on image substitution
        Make raw html from markdown
        Insert images into raw html
        Work in JupyterLab to convert the configuration file into raw html

# Tasks

    Clean up plugins in vimrc

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
