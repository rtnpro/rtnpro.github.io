+++
author = "Ratnadeep Debnath"
categories = ["cedega", "crossover games", "Fedora", "linux", "playonlinux", "ratnadeep debnath", "wine", "witcher", "rtnpro"]
date = 2009-05-22T13:09:36Z
description = ""
draft = false
slug = "install-windows-games-in-linux-with-playonlinux"
tags = ["cedega", "crossover games", "Fedora", "linux", "playonlinux", "ratnadeep debnath", "wine", "witcher", "rtnpro"]
title = "Install Windows Games in Linux with PlayOnLinux ..."

+++


Hi all… I have a Dell XPS M1530 running on Fedora 10. It has got a 2 GHz Intel Centrino Duo processor, 2GB RAM, 256 MB Nvidia GeForce 8600M GT, quite a configuration for decent gaming. But till today I had to boot in Windows Vista to play games. So I have been trying to install and run Windows games in Fedora. There are a few tools for this purpose. The famous and proprietory and non-free Transgaming Cedega, Codeweavers Crossover Games and the all free PlayOnLinux. All these use wine which is a free software for running Windows applications in Linux. Though paid, Cedega and Crossover Games don’t support as many applications as PlayOnLinux. And when you are not sure that whether your games are going to run or not in Linux, wine and PlayOnLinux is the free and good option to stick to.

PlayOnLinux has got a lot of .pol scripts to automate the installation of games, but they require that the game’s installation source is in a cd/DVD ROM. I tried installing from the game ISO mounted or from any local folder, but failed till yesterday. I did a bit google and tried to read the .pol file and found that games can be installed from a source folder of the game without it being on a cdrom. Here’s how to do it:

1. Open playonlinux, its better if you open it from the terminal, it gives many useful information :

> $playonlinux

2. Select ***Install***.

3. Then select “***Install a .pol package or an unsupported application***“.

4. If you don’t have any custom **.pol **package select “***Manual installation***” and click “***Next***” .

5. Do as directed, then under “***What do you want to do***?” select “***Install a program in a new prefix***”  in case you are installing the application for the first time. Select “***Edit an application already installed***” if you have to update your installed application like patching it to a newer version, or installing mods, or other packs, or may be other support softwares like DirectX, etc.

Select “Delete a prefix” if you want to delete an already installed application (wineprefix)

6. Do as directed.

7. “Do you want to confiure your prefix before installing your application?”

If you want to configure wine before installtion, select “Yes”, otherwise “No”

8. “Browse the file to execute”

Browse the setup.exe (or whatever for your game) in the game source folder, and select Forward.

9. Then follow the onscree instructions and finish the installation, and allow creating shortcuts (if you want…)

10. Now you will be able to see the installed game in Playonlinux window. Select the game, and choose “***Configure this application***“. Use the “***Use advanced wine configuration plugin***” to do some wine tweaks. You get the options of configuring

- *DirectDrawRenderer*
- *UseGLSL*
- *VideoMemorySize*
- *OffscreenRenderingMode*
- *RenderTargetLockMode*
- *Multisampling*
- *MouseWarpOverride*

After configuring these based on the hardware abilities on your system, you are almost done.

11. But still we don’t have DirectX installed and without these, the DirectX requiring games will fail to run. You can either Install DirectX manually via PlayOnLinux and choose the concerned prefix of the game which you want to update.

Or you can just copy some dll files from the Windows/System32 folder if you have Windows installed in your system, or you can get from your friend’s computer running Windows and having DirectX 9.

Playonlinux has got a lot of supported games, and you always have the option of adding new games to the list.

