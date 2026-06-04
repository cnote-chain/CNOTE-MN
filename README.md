# CNOTE Masternode Setup:
This guide will assist you in setting up a CNOTE Masternode on a Linux Server running Ubuntu VPS.

- [CNOTE Masternode Setup](#CNOTE-masternode-setup)  
  	* [Requirements](#requirements) 
  * [Connecting to the VPS and installing the masternode script](#Connecting-to-the-VPS-and-installing-the-masternode-script)  
         [1. Log into the VPS with **root**](#1-log-into-the-vps-with-root)  
         [2. Git Installation](#2-git-installation)  
         [3. Clone MN setup script](#3-clone-mn-setup-script)  
         [4. Start MN setup script](#4-start-mn-setup-script)  
         [5. Copy Masternode Private Key](#5-copy-masternode-private-key-from-vps-console-window-and-pres-enter)
  * [Setup QT wallet](#setup-qt-wallet)  
         [1. Create new receiving address and copy it](#1-create-new-receiving-address-and-copy-it)  
	 [2. Send Collateral amount of CNOTE to copied address](#2-send-collateral-amount-of-CNOTE-to-copied-address)  
	 [3. Get MN output and Set Masternode Configuration File](#3-open-console-get-mn-output-and-set-masternode-configuration-file-and-save-it)  
	 [4. Wait at least 15 confirmation of transaction](#4-wait-at-least-15-confirmation-of-transaction)  
         [5. Restart QT wallet](#5-restart-qt-wallet)  
         [6. Start MN in QT wallet console](#6-start-mn-in-qt-wallet-console)  
	 [7. Check Masternode Status in VPS](#7-check-masternode-status-in-vps)  
  * [Guide for CNOTE v2.0.0 Masternode Update](#guide-for-cnote-v200-masternode-update)  
         [1. Back up your existing masternode private key](#1-back-up-your-existing-masternode-private-key)  
         [2. Run the update script](#2-run-the-update-script)  
         [3. Enter your existing masternode private key](#3-enter-your-existing-masternode-private-key)  
         [4. Check Masternode Status after update](#4-check-masternode-status-after-update)  

## Requirements
- MN Collateral amount of CNOTE coins.
- A VPS running Linux Ubuntu 18.04/20.04/22.04 with 1 CPU & 1GB Memory minimum (2gb Recommended) from [Vultr](https://www.vultr.com/?ref=8622028) or any other providers.
- CNOTE Wallet (Local Wallet)
- An SSH Client (<a href="https://www.putty.org/" target="_blank">Putty</a> or <a href="https://dl.bitvise.com/BvSshClient-Inst.exe" target="_blank">Bitvise</a>)


## Connecting to the VPS and installing the masternode script

##### 1. Log into the VPS with **root**  

##### 2. Git Installation:  
- ```sudo apt-get install -y git-core```  

##### 3. Clone MN setup script: 
- ```git clone https://github.com/cnote-chain/CNOTE-MN.git```  

##### 4. Start MN setup script: 
##### For Ubuntu 18.04, 20.04 & 22.04
- ```cd CNOTE-MN && chmod +x ./CNOTE-MN.sh && ./CNOTE-MN.sh```
   
**Now ask for VPS Public IP Address** 

**Now you need to wait some time, while script preparing the VPS to setup**  
##### 5. Copy masternode private key from VPS console window and pres "Enter":


- to check VPS daemon status, type: ```cnote-cli getinfo```

**Don't close this window!** 	

## Setup QT wallet
##### 1. Create new receiving address and copy it

##### 2. Send Collateral amount of CNOTE to copied address

##### 3. Open console Get MN output and set masternode configuration file and save it
- ```mn1 VPS_IP:24860 masternode_genkey masternode_output output_index```:

##### 4. Wait at least 15 confirmation of transaction

##### 5. Restart QT wallet  
- **it's important**

##### 6. Start MN in QT wallet console:
- ```startmasternode alias false TEST-MN```

##### 7. Check Masternode Status in VPS:
- ```cnote-cli startmasternode local false``` 
- ```cnote-cli getmasternodestatus```  

**Сongratulations you did it!**

## Guide for CNOTE v2.0.0 Masternode Update

These instructions are for users who are **already running an older CNOTE masternode** and need to upgrade their VPS to **v2.0.0**.  
The update script stops the running daemon, installs the new v2.0.0 binaries, refreshes the sapling params and re-syncs from bootstrap. As long as you re-use your existing masternode private key, your node keeps the same identity and you do **not** need to reconfigure it from your QT wallet.

> **Important:** Run all commands on the VPS, logged in as **root**.

##### 1. Back up your existing masternode private key
You will need it during the update. Print it and copy it somewhere safe:
- ```cat ~/.cnote/cnote.conf | grep masternodeprivkey```

##### 2. Run the update script
##### For Ubuntu 18.04, 20.04 & 22.04
Execute these commands in sequence:
```
rm -rf CNOTE-MN-v2.sh

wget -q https://raw.githubusercontent.com/cnote-chain/CNOTE-MN/main/CNOTE-MN-v2.sh

sudo chmod +x CNOTE-MN-v2.sh

./CNOTE-MN-v2.sh
```

##### 3. Enter your existing masternode private key
When the script asks **"Do you want me to generate a masternode private key for you?[y/n]"**, type **`n`** and paste the masternode private key you backed up in step 1 (you will be asked to confirm it). This keeps your masternode on the same key, so it stays in the masternode list.

**Now you need to wait some time, while the script updates the VPS and re-syncs the chain.**

##### 4. Check Masternode Status after update
- to check daemon status, type: ```cnote-cli getinfo```
- to check masternode status, type: ```cnote-cli getmasternodestatus```

The status may initially read `Node just started, not yet activated` or `Node is not in masternode list` — this is normal while the node finishes syncing. Once `Is Synced` is `true`, the status should return to enabled.

**Update complete!**