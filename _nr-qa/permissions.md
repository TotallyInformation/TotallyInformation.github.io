---
title: File and folder permissions
description: >
    People seem to get very confused about file permissions when installing Node-RED and associated nodes.

    This article tries to set things straight.
comments: true
date: 2018-01-06 22:00:00
---

It is easy to get confused about the correct permissions but the rules are really quite simple.

## Quick summary

For standard installations, <b><u>do</u></b> install Node-RED itself globally with admin rights (`sudo`, except on Windows where you use standard user rights unless really needing to install for all users) but <b><u>do not</u></b> install nodes globally.

If you make a mistake and install a node with admin rights, uninstall it and reinstall without. You may, however, need to check your `userDir` folder (normally `~/.node-red`) to make sure that the access control privileges for all folder are correct.

## Linux (and probably Mac)

See also the [official documentation](https://nodered.org/docs/getting-started/installation).

### Dependencies

Unless you can use the official installation and upgrade script, you must ensure that all dependencies are installed. This includes Node.JS and npm. Simply follow the instructions on the Node.JS website for your distribution or operating system to get the latest "LTS" (Long Term Support) version of Node.JS that is supported by Node-RED (v6 or v8 at the time of writing). You should not rely on the version of Node.JS that comes with the default distribution libraries as this is typically long out of date.


### Installation of Node-RED

There are two types of installation for Node-RED.

#### The standard installation

The standard installation is generally recommended. It <b><u>is</u></b> installed with admin privalages:

```bash
sudo npm install node-red -g
```

In this case, you can run Node-RED from anywhere with a command that is globally available:

```bash
node-red
```

The resulting process runs with the permissions of the logged in user, <b><u>not</u></b> with admin or root permissions.

Commonly though, you may well want to run Node-RED as a system process, a "daemon" in UNIX terminology. Generally, you should make sure that the resulting process does not start until after networking and any other dependent processes (such as databases) are active. You should also ensure that the process runs with the privalages of a specific, non-admin user.

To upgrade Node-RED, you should use the upgrade script provided if running under Debian/Ubuntu/Rasbian type versions of Linux. This ensures that all dependencies are met. Note that, as is typical with many Linux installations, any default versions of Node.JS and Node-RED available from core Linux distributions are likely to be out of date and should rarely be used. If you cannot use the upgrade script, simply run `sudo npm upgrade node-red`.

#### The "embedded" installation

The alternative to the standard installation is an embedded installation. You can use this either to embed in a large app or to allow multiple versions of Node-RED to run in parallel.

For embedded installations, you should <b><u>not</u></b> install Node-RED with admin privalages.

```bash
mkdir project-folder
cd project-folder
npm init -y
npm install node-red --save
```

The `init` and `--save` parts ensure that the folder contains a `package.json` file that will help you manage dependencies correctly.

Note that, you will need to manage the location of your `userDir` folder for this installation type. Typically, you will not want to use `~/.node-red` but probably a sub-folder of the `project-folder`. See the Node-RED documentation for how to do this.

While this is harder to set up, it provides a much more flexible installation.

To upgrade Node-RED, you simply manage it like any other dependent Node.JS module.

```bash
cd project-folder
npm update node-red
```

### Installation of nodes

Nodes are simple Node.js npm modules. They should **always** be installed with <b><u>standard</u></b> user permissions. The user in question needs to be the id that is running the Node-RED process.

By far the best way to install nodes is from within the Node-RED admin interface. Click on the "hamburger" menu and select "Manage pallette". This will always install nodes to the correct folder (the `userDir` folder, typically `~/.node-red`) where you can see the code in the `node_modules` folder (e.g. `~/.node-red/node_modules/`).

Alternatively, you can install from the command line but must make sure you are in the correct location, the `userDir` folder as above. Don't forget that, if you have used an embedded installation of Node-RED, the `userDir` folder will most likely be somewhere non-standard but you should still install nodes there even though installing them in the same folder as Node-RED itself will certainly work for most nodes (but that wouldn't let you manage them from the Admin interface).

```bash
cd ~/.node-red
npm install node-red-contrib-uibuilder --save
```

## Windows

See also the [official documentation](https://nodered.org/docs/platforms/windows).

### Installation of Node-RED

For modern versions of Windows, you should generally <b><u>not</u></b> use admin privileges to install Node-RED even when installing globally unless you really need to install it for **all** users. However, when you don't, you do not actually get a global installation in the same way as Linux. Instead it is installed to `%APPDATA%\npm` (or `$env:APPDATA\npm` for PowerShell users) for the installing user. This is more secure, you can give permissions to other users if you need to.

To upgrade, use `npm upgrade node-red -g` as with other globally installed Node.JS modules.

You can, of course, also install in embedded style in the same way as for Linux.

### Installation of nodes

With a standard installation,
