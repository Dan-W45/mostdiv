+++
title = 'Satisfactory Server'
date = 2024-01-20T09:38:58Z
draft = false
[cascade]
	type = 'blog'
+++

> [!NOTE]
All new servers are installed through [Pterodactyl](../pterodactyl) and Wings

I have this installed in a Debian Linux VM, a Satisfactory server is
installed via steamcmd. path may not be configured to launch steamcmd
from just the command line so cd around link it up or something then
go back to the home folder for steam.


```markdown {filename="Satisfactory save files location:"}
/home/steam/.config/Epic/FactoryGame/Saved/SaveGames/server
```

```markdown {filename="Satisfactory server:"}
/home/steam/.steam/steamapps/common/SatisfactoryDedicatedServer
```

```markdown {filename="Launch server:", linenos=table,hl_lines=[2,4]}
sudo -u steam -s
cd /home/steam/SatisfactoryDedicatedServer
./FactoryServer.sh
./FactoryServer.sh -multihome=192.168.1.106
```

```markdown {filename="Update/Install Server:"}
steamcmd +force_install_dir ~/SatisfactoryDedicatedServer +login anonymous +app_update 1690800 -beta public validate +quit
```

> [!IMPORTANT]
See SteamCMD blog post [here](../steamcmd)
