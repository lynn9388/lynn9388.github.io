---
title: Switch IP Quickly
tags: [Network, macOS, Windows]
---

I usually need to access different networks through different network settings, such as accessing the campus network via DHCP and using static IP to access the server in the lab. So I need a way to switch IP quickly.

## For macOS

macOS supports saving multiple network settings for different locations. You can see the settings in `System Preferences -> Network -> Location`.

![Network settings for different locations](https://lynn9388.github.io/images/post/Network_settings_for_different_locations.png)

### Add New Locations

You just need to click `Location -> Edit Locations... -> +`, and set a name for the untitled new location. For example, I set two locations, `Automatic` and `lab`. The former uses the default network settings to enable Wi-Fi to automatically obtain IP, while the latter will set a static IP, which allows me to access resources on the LAN.

![Add a new location for network](https://lynn9388.github.io/images/post/Add_a_new_location_for_network.png)

### Switch Locations

After you add a new network, you will find a new option in the menu of the Apple logo in the upper left corner of the screen. All you need to do is switch between different locations when you need to change the IP quickly.

![Change IP quickly by switching locations](https://lynn9388.github.io/images/post/Change_IP_quickly_by_switching_locations.png)

## For Windows

Windows users could quickly configure the network through [Network Shell (Netsh)](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh). For convenience, you can create a batch file and choose to execute different commands when you need to switch IP.

### Create Batch File

You can create a batch file (such as `config-ip.bat`) based on the code below (You can also view the source file [here](https://github.com/lynn9388/script-tools/blob/master/windows/config-ip.bat)).

```bat
REM Please update the variables below before you run it
SET ethernet-adapter-name=Ethernet
SET wifi-adapter-name=Wi-Fi
SET ethernet-static-ip=192.168.1.90
SET wifi-static-ip=192.168.1.90

@echo off
cls
color 0A

:check_Permissions
fsutil dirty query %systemdrive% >nul
if %errorLevel% == 0 (
    goto loop
) else (
    echo ######
    echo #     # #      ######   ##    ####  ######    #####  #    # #    #      ##    ####
    echo #     # #      #       #  #  #      #         #    # #    # ##   #     #  #  #
    echo ######  #      #####  #    #  ####  #####     #    # #    # # #  #    #    #  ####
    echo #       #      #      ######      # #         #####  #    # #  # #    ######      #
    echo #       #      #      #    # #    # #         #   #  #    # #   ##    #    # #    #
    echo #       ###### ###### #    #  ####  ######    #    #  ####  #    #    #    #  ####
    echo=
    echo    #    ######  #     # ### #     # ###  #####  ####### ######     #    ####### ####### ######     ### ###
    echo   # #   #     # ##   ##  #  ##    #  #  #     #    #    #     #   # #      #    #     # #     #    ### ###
    echo  #   #  #     # # # # #  #  # #   #  #  #          #    #     #  #   #     #    #     # #     #    ### ###
    echo #     # #     # #  #  #  #  #  #  #  #   #####     #    ######  #     #    #    #     # ######      #   #
    echo ####### #     # #     #  #  #   # #  #        #    #    #   #   #######    #    #     # #   #
    echo #     # #     # #     #  #  #    ##  #  #     #    #    #    #  #     #    #    #     # #    #     ### ###
    echo #     # ######  #     # ### #     # ###  #####     #    #     # #     #    #    ####### #     #    ### ###
    pause
    Exit
)

:loop
echo 1 DHCP %ethernet-adapter-name%
echo 2 Static %ethernet-adapter-name% (%ethernet-static-ip%)
echo 3 DHCP %wifi-adapter-name%
echo 4 Static %wifi-adapter-name% (%wifi-static-ip%)
echo 5 Show IP Configuration
echo 6 Exit
echo|set /p=Please choose a option:

set/p sel=
if "%sel%"=="1" goto DHCP-Ethernet
if "%sel%"=="2" goto Static-Ethernet
if "%sel%"=="3" goto DHCP-Wifi
if "%sel%"=="4" goto Static-Wifi
if "%sel%"=="5" goto Show-IP-Configuration
if "%sel%"=="6" EXIT
echo=
echo Please choose a valid option
goto loop

:DHCP-Ethernet
netsh interface ip set address name=%ethernet-adapter-name% source=dhcp
netsh interface ip delete dns %ethernet-adapter-name% all
ipconfig /flushdns
goto Show-IP-Configuration

:Static-Ethernet
netsh interface ip set address name=%ethernet-adapter-name% source=static addr=%ethernet-static-ip% mask=255.255.255.0 gateway=192.168.1.1
netsh interface ip set dns name=%ethernet-adapter-name% source=static addr=192.168.1.1
ipconfig /flushdns
goto Show-IP-Configuration

:DHCP-Wifi
netsh interface ip set address name=%wifi-adapter-name% source=dhcp
netsh interface ip delete dns %wifi-adapter-name% all
ipconfig /flushdns
goto Show-IP-Configuration

:Static-Wifi
netsh interface ip set address name=%wifi-adapter-name% source=static addr=%wifi-static-ip% mask=255.255.255.0 gateway=192.168.1.1
netsh interface ip set dns name=%wifi-adapter-name% source=static addr=192.168.1.1
ipconfig /flushdns
goto Show-IP-Configuration

:Show-IP-Configuration
netsh interface ip show addresses %ethernet-adapter-name%
netsh interface ip show addresses %wifi-adapter-name%

echo=
goto loop
```

**Note** that before you run the batch file you should update the contents of the file at the start based on your network adapters and IP. If your settings for LAN are different from mine, you may need to modify other network parameters.

### Run Batch as Administrator

When you want to switch IP, you only need to run the batch file you created earlier and then choose one option. But be aware that you must run it as administrator, otherwise you will only see one message `PLEASE RUN AS ADMINISTRATOR !!`.

BTW, you can avoid manually select `Run as administrator` from context menu by following steps below

1. Create a shortcut for this batch
1. Click `Properties` from the context menu to open `Shortcut Properties`
1. Click `Advanced` from the `Shortcut` tab
1. Check `Run as administrator` in the pop-up window
1. Save and close pop-up windows

After that, you only need to double-click the shortcut to run the batch file as administrator. Enjoy!😃
