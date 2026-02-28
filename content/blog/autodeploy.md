+++
title = 'GitHub Action Autodeploy'
date = 2026-02-28T15:25:31Z
draft = false
+++

# What
I need a way to pull from GitHub when new content has been pushed to the repo.
The builtin method for GitHub is actions which be used to run a script when
certain conditions are met, like a push to the main branch.

# Why
Simply I wanted a way that when I pushed this website to GitHub, the server would
automatically pull the latest version compile and then host the site. A cron job
can do this on a set schedule, but for the way that I work that would be wastful.

# How

### Server Setup
1. Create a new user with no sudo access
`sudo adduser <USERNAME>`
1. Create SSH keys:
`ssh-keygen -t rsa -b 4096` <br>
No additional information is needed, you can just press enter at each question
1. Add the public key to the authorized_keys file:
`cat /root/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`
1. Copy private key into your clipboard:
`cat /root/.ssh/id_rsa` <br>
This is needed by the GitHub Action
1. To increase the security of allowing SSH connections from WAN the config
can be edited to disable logging in with a password unless the connection is
comming from a LAN address
    ```markdown {filename="/etc/ssh/sshd_config.d/50-cloud-init.conf"hl_lines=[6]}
	#Default config
	PasswordAuthentication no

	AuthenticationMethods publickey

	Match Address 192.168.1.0/24
			PasswordAuthentication yes
			AuthenticationMethods publickey password
	```
	> [!CAUTION]
	Pay attention to the fact the the file name/path may be slightly different
	on your machine and the LAN address range might be different


### GitHub Setup

1. Go to “Settings > Secrets and Variables > Actions”
1. Create the following secrets:
	* `SSH_PRIVATE_KEY` Private key that shoule be in your clipboard
	* `SSH_USER` non sudo user created earlier
	* `SSH_HOST` hostname/ip-address of your server
	* `SSH_PORT` port to connect over (default 22)
1. Go to the “Actions” tab in your GitHub repository
1. Click on “set up a workflow yourself →”

```yml {filename="main.yml"}
on:
  push:
    branches:
      - main
  workflow_dispatch:
  
jobs:
  run_pull:
    name: run pull
    runs-on: ubuntu-latest
    
    steps:
    - name: install ssh keys
      # check this thread to understand why its needed:
      # https://stackoverflow.com/a/70447517
      run: |
        install -m 600 -D /dev/null ~/.ssh/id_rsa
        echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_rsa
        echo "${{ secrets.SSH_HOST }}" > ~/.ssh/my_hosts
        grep -h "" ~/.ssh/my_hosts | xargs -r -n1 ssh-keyscan -H -p ${{ secrets.SSH_PORT }} >> ~/.ssh/known_hosts
    - name: connect and pull
      run: |
        echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/machine.key
        chmod 600 ~/.ssh/machine.key
        ssh -i ~/.ssh/machine.key ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }} -p ${{ secrets.SSH_PORT }} "./deploy.sh && exit"
    - name: cleanup
      run: rm -rf ~/.ssh

```

### Additional Notes

The code may not be the most effecient, but it works after many trial and error
attempts.

```markdown {filename="deploy.sh"}
cd ~/mostdiv
git checkout main
git pull
hugo
```

In getting this to work some possibly silly things were done with permissions
including doing `chmod -R 777 ~/` in an attempt to get nginx to read files in
the users home directory

> [!CAUTION]
It is imperative that the home folder has `755` or stricter permissions set or
you will get <nobr>`Permission denied (publickey).`</nobr> even if all else is correct.

Also of note:
`chmod 700 ~/.ssh`
`chmod 600 ~/.ssh/authorized_keys`