# Adding a Second Conga to HA addon

This guide will help you add another robot to your Home Assistant (HA) server.

## **Prerequisites**
- Basic knowledge of the Congatudo project.
- Root access to your Conga robot via [SSH or ADB](https://github.com/congatudo/Congatudo/tree/master#get-root-access-in-your-conga).
- A Windows computer (anything from the last 15 years is good enough.)

## Table of Contents

1. [Install WinSCP](#1-install-winscp)
2. [Connecting to your Conga](#2-connecting-to-your-conga)
3. [Create a Folder for Safe Keeping](#3-create-a-folder-for-safe-keeping)
4. [Editing Hosts File to Default](#4-edit-the-hosts-file-to-default)
5. [Navigate and Edit sysConfig.ini](#5-navigate-and-edit-sysconfigini)
6. [Installing and Configuring Congatudo Beta Addon in HA](#6-install-and-configure-the-congatudo-beta-addon-in-home-assistant)
7. [Credits](#credits)



## **1. Install WinSCP**
1. Download and install [WinSCP](https://winscp.net/).
2. Follow the setup instructions.

## **2. Connecting to Your Conga**
1. Open WinSCP.
2. Create a new session with the following details:
   - **File Protocol**: SCP
   - **Host Name**: (your.conga.ip.address)
   - **Port Number**: 22
   - **User Name**: root
   - **Password**: (your password, or the default if unchanged)
3. Click **Login**.

## **3. Create a Folder for Safe Keeping**
1. By default, the left window in WinSCP shows your Windows OS directories, and the right window shows your Conga’s files.
2. Create a new folder called `Conga` under your Documents folder.
3. Open the `Conga/` folder and drag and drop the `/etc/` folder into it to create a backup.

### Optional Backup Method
Alternatively, open the terminal (Shift+Ctrl+T) and type:
```bash
tar -cvpzf /tmp/etc.tar.gz /etc/
```
And don't forget to fetch the `etc.tar.gz` into somewhere safe.

## **4. Edit the `hosts` File to Defaults**
If you have already redirected traffic from your Conga to your HA server, you must reset it:

1. Navigate to `/etc/hosts`.
2. Delete the following lines:
   ```plaintext
   <your server ip> cecotec.das.3irobotix.net cecotec.download.3irobotix.net cecotec.log.3irobotix.net
   cecotec.ota.3irobotix.net eu.das.3irobotics.net eu.log.3irobotics.net eu.ota.3irobotics.net
   cecotec-das.3irobotix.net cecotec-log.3irobotix.net cecotec-upgrade.3irobotix.net
   cecotec-download.3irobotix.net
   ```
   
## **5. Navigate and Edit `sysConfig.ini`**

1. Navigate to `/etc/sysconf` and open `sysConfig.ini`.
2. Locate and modify the following lines:
   ```ini
   server_cmd_address=cecotec.das.3irobotix.net
   server_map_address=cecotec.das.3irobotix.net
   server_log_address=cecotec.log.3irobotix.net
   server_ota_address=cecotec.ota.3irobotix.net
   server_down_address=cecotec.download.3irobotix.net

   server_cmd_port=4010
   server_map_port=4030
   server_sync_time_port=4050
   ```
Then replace `cecotec.xxxxx.3irobotix.net` with the IP address of your HA :
   ```ini
   server_cmd_address=192.168.1.10
   server_map_address=192.168.1.10
   server_log_address=192.168.1.10
   server_ota_address=192.168.1.10
   server_down_address=192.168.1.10

   server_cmd_port=4011
   server_map_port=4031
   server_sync_time_port=4051
   ```
3. Save and close the file.
4. Open a terminal in WinSCP (press Shift + Ctrl + T) and reboot the robot by typing:
   ```bash
   reboot
   ```
   Note: WinSCP will become unresponsive until the Conga robot finishes rebooting and comes back online.

## **6. Install and Configure the Congatudo Beta Addon in Home Assistant**

1. Open **Home Assistant** and navigate to:
   - **Settings** → **Devices & Services** → **Add-ons** → **Add-on Store**.
2. Locate and install **Congatudo (Beta)** addon.
3. After installation, open the **Configuration** tab of the addon and adjust the port numbers to match the ones set earlier:
   - `server_cmd_port=4011`
   - `server_map_port=4031`
   - `server_sync_time_port=4051`
4. In the **Info** tab:
   - Enable **Show in sidebar**.
   - Click **Start** to start the addon.

## Expected Log Output
If everything was configured correctly, you should see logs similar to the following in the Log tab:
```pgsql
[2023-12-26T16:48:33.108Z] [INFO] Webserver running on port 8080
[2023-12-26T16:48:33.560Z] [INFO] Connected successfully to MQTT broker
[2023-12-26T16:48:34.031Z] [INFO] MQTT configured
[2023-12-26T16:48:34.707Z] [INFO] Added new robot with id 'xxxxx'
```




## Credits

A special thanks to:

- **@Perry_drum** for confirming this approach.❤️
- **@Jesús** for the addon.❤️


