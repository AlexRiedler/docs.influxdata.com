---
title: Installation
menu:
  telegraf_013:
    weight: 10
    parent: introduction
---

This page provides directions for installing, starting, and configuring Telegraf.

## Requirements

TODO: More here

### Networking

TODO: More here

## Installation

TODO: More here

### Start the Telegraf service

TODO: Confirm

#### OS X (via Homebrew)
To have launchd start telegraf at login:
```
ln -sfv /usr/local/opt/telegraf/*.plist ~/Library/LaunchAgents
```
Then to load telegraf now:
```
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.telegraf.plist
```

Or, if you don't want/need launchctl, you can just run:
```
telegraf -config /usr/local/etc/telegraf.conf
```

#### sysv systems
```
sudo service telegraf start
```

#### systemd systems (such at Ubuntu 15+)
```
systemctl start Telegraf
```

## Configuration

### Create a configuration file with default input and output plugins.

Every plugin will be in the file, but most will be commented.

```
telegraf -sample-config > telegraf.conf
```

### Create a configuration file with specific inputs and outputs
```
telegraf -sample-config -input-filter <pluginname>[:<pluginname>] -output-filter <outputname>[:<outputname>] > telegraf.conf
```

For more advanced configuration details, see the
[configuration documentation](/telegraf/v0.13/administration/configuration/).