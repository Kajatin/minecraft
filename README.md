# Minecraft Server

This is a repo with information about setting up and running my own Minecraft server.

The setup is mainly based on the following sources:

1. https://minecraft.fandom.com/wiki/Tutorials/Setting_up_a_server#Common_instructions
1. https://help.minecraft.net/hc/en-us/articles/360058525452-How-to-Setup-a-Minecraft-Java-Edition-Server

I'm currently using the [Paper Minecraft](https://papermc.io/downloads/paper) server `.jar`.
There are a bunch of parameters to the Java runtime. I'm using the following launch command:

```bash
java -Xms8G -Xmx8G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar paper-1.19.4-545.jar --nogui
```

For enhanced security, the user running the server and owning the files is one without
`sudo` priviliges. Here's how you can create a new user and own the files:

```bash
sudo adduser minecraft
sudo chown -R minecraft:minecraft /home/home-server/minecraft/worlds
```

I've also configured my server to be whitelist-only. New users can be added as: `whitelist add Kajatin`.

## Setting up a new world

The worlds are placed inside the `worlds/` folder, which is not tracked on Git.
To set up a new world, follow these steps:

1. Create a new world folder inside `worlds/`.
1. Copy the server `.jar` file in there and run it.
1. Accept EULA.
1. (Optional) Copy `server.properties.example` or adjust server configuration to your liking.
1. (Optional) Modify and/or install the `systemd` service file.
1. Run the server (or start the `systemd` service).

## Server properties

There are some parameters that control what kind of world is generated. There is an
example configuration in `server.properties.example`, but there's more information on
[the official wiki](https://minecraft.fandom.com/wiki/Server.properties).

## `systemd` service

Set up a `systemd` service to launch the server automatically. The service configuration
is set in `minecraft.service` and can be installed as:

```bash
sudo ln -s /home/home-server/minecraft/minecraft.service /etc/systemd/system/minecraft.service
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
```

## Reverse proxy configuration

TBD
