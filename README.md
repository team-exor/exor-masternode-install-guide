# Exor Masternode Install Guide
This tutorial details the steps required to set up an Exor masternode wallet on Ubuntu 16.04+ or Debian 8.x+ x64 with a local controller wallet. The steps in this tutorial were written for the Windows controller wallet but should be similar enough for MacOS and Linux. The local controller wallet will store the actual currency and can be taken offline for security purposes if desired, while the actual masternode wallet on Linux will do all the work required to generate a passive income. 
## Requirements
1) **50,000 Exor** (you actually require a tiny bit more than this to cover the cost of transfer fees - 50,001 is perfect)
2) **VPS server running Ubuntu 16.04+ or Debian 8.x+ x64** (bare minimum of 10GB Hard Drive, 512MB Memory)
3) **Local Windows/MacOS/Linux Exor Wallet**
4) **An SSH client such as [Putty](https://www.putty.org/)**
## Table of Contents
- [Pre-Setup Local Wallet](#pre-setup-local-wallet)
- [Pre-Setup VPS](#pre-setup-vps)
- [VPS Wallet Install](#vps-wallet-install)
- [Final Local Wallet Setup](#final-local-wallet-setup)
- [VPS Wallet Update](#vps-wallet-update)
- [Verify Masternode Setup (Optional)](#verify-masternode-setup-optional)
## Pre-Setup Local Wallet
**1) Create new wallet address:**
- Open the debug console from the main menu (Tools > Debug Console)
- Type the following cmd and press [ENTER]: `getaccountaddress MN1`
- **NOTE:** `MN1` should be unique and can be changed to virtually anything that you want. Be sure to remember what you enter here for that value as it will be required later in the setup process. This value will be referred to as the `wallet_alias` for the remainer of this document.
- After submitting the `getaccountaddress` cmd, the new public wallet address should be returned. Make sure to copy this address and save it in a safe place for now such as a new notepad window. This value will be referred to as the `wallet_address` for the remainer of this document.

**2) Send exactly 50,000 EXOR to the new masternode address:**
- Click into the `Send` tab and paste the `wallet_address` you just copied from last step into the `Pay To` box.
- **NOTE:** This will automatically populate the `Label` box with your `wallet_alias` (`MN1` in this example).
- Enter __**exactly**__ 50000 into the `Amount` box and ensure that the dropdown beside is set to `EXOR`.
- Click the `Send` button and click `Yes` when it asks if you are sure you want to send 50000 EXOR to your masternode address.
-	If your wallet is encrypted (it should be!) and locked, it will now ask you for your unlock passphrase. Enter the passphrase and click `OK`.
-	You may see a final message asking you to confirm the transaction fee. Click `Yes` to finalize the transaction.
-	**NOTE:** After sending the 50,000 masternode coins to yourself, your wallet will begin trying to stake that large amount of coins in a short amount of time, which can cause the coins to become immature and unmovable for a few hours. It is therefore recommended at this point that you either lock your wallet if it is encrypted, which will prevent staking from occurring, or else you should completely shut down the wallet software for now until needed again later in the process.

There are still a few more steps needed to complete the local wallet setup, but this concludes the pre-setup instructions.

## Pre-Setup VPS
**NOTE:** If you already have a VPS configured the way you want, you may skip these pre-setup steps, but they are recommended for maximum security.

**1) Create a new user account:**
- Log into the VPS as the `root` user using putty on Windows or the built-in SSH app in MacOS or Linux
- Type the following cmd and press [ENTER]: `adduser masternodeuser`
- **NOTE:** `masternodeuser` is the name of the new user account that you are creating. It can be virtually anything that you want as long as it doesn't include certain special characters
- You will be prompted to enter a password for the new user account. Typically there are restrictions on the password where it must meet certain criteria such as have one uppercase, one lowercase, one number and one special character for example. Enter the password
- You will be prompted to renter the password once again to confirm. Enter the password again
- You will be prompted to fill in extra information about the user such as the full name, room number, phone numbers, etc. None of these fields are required and can be skipped by simply pressing [ENTER] for each question
- The last question that will be asked is: `Is the information correct? [Y/n]`. Type `Y` and press [ENTER] to finish creating the new user account

**2) Grant super user access to the new user account:**
- Type the following cmd and press [ENTER]: `usermod -aG sudo masternodeuser`
- **NOTE:** `masternodeuser` is the name of the new user account that you created above. If you changed the name you will need to ensure you type the same username in this cmd as well

**3) Log off from the VPS as the root user:**
- Type the following cmd and press [ENTER]: `exit`
- **NOTE:** This will end your SSH session and logoff from the VPS. As an alternative, you can also click the 'X' on the window to close the connection as well

## VPS Wallet Install

**1) Download the Exor Masternode Install Script:**
- Log into the VPS as the new user account created in the `Pre-Setup VPS` instructions. (`masternodeuser` in this example)
- Type the following cmd and press [ENTER]: `wget https://raw.githubusercontent.com/team-exor/exor-mn-installer/master/exor-mn-installer.sh`
- Type the following cmd and press [ENTER]: `chmod +x exor-mn-installer.sh`

**2) Install the Linux Masternode Wallet:**
- **NOTE:** The masternode install script supports a number of different options, but the most common choice to make during the install is whether to install the wallet with support for IPv6 or IPv4. IPv6 is the default setting and is essentially required if you are going to be installing more than one Exor masternode on the same VPS. Depending on the VPS provider, you may or may not have the ability to create unlimited IPv6 addresses for free. Vultr is recommended for multiple IPv6 installs as they do provide unlimited free IPv6 addresses, as long as you choose to enable IPv6 during the VPS creation process. IPv4 addresses are more familiar to most people but typically incur an additional cost and possibly extra steps to setup more than one per VPS.
- **DO NOT RUN BOTH CMDS BELOW. CHOOSE WHICH OPTION IS CORRECT FOR YOU AND RUN THE CHOSEN CMD ONE TIME ONLY**
- To install an IPv6 supported wallet, type the following cmd and press [ENTER]: `sudo sh exor-mn-installer.sh`
- To install an IPv4 supported wallet, type the following cmd and press [ENTER]: `sudo sh exor-mn-installer.sh -N 4`
- **NOTE:** The wallet install process is completely automated and can take several minutes to complete. Please be patient while your masternode wallet is installed and configured

**NOTE:** Before your masternode will start properly, it must be fully synchronized with the network. Depending on which block the network is currently processing, syncing the masternode wallet can take a few minutes to a few hours. You do not have to wait for the sync to finish before proceeding. If you do continue without waiting for a full sync, please keep it in mind for later in case you are having troubles verifying that the masternode was started correctly.

At the end of the masternode install process you will find some useful cmds and final setup instructions (you may have to scroll up in the terminal window after the install is complete). The important bit here is the `masternode.conf file setup` instructions. Either copy this info into a new notepad window or else just keep the SSH terminal window open for easy access while you finish the last steps below.

## Final Local Wallet Setup

**1) Find the Masternode Output Values:**
- Back in the local controller wallet you can open the debug console from the main menu (Tools > Debug Console)
- Type the following cmd and press [ENTER]: `getmasternodeoutputs`
- You will be presented with output that looks similar to this:
```
[
  {
    "txhash": "813656a0465ce757f7a02c03c0e4a6174f6d7c4c021d920d4a47cbdaa1045c02",
    "outputidx": 0
  }
]
```
- **NOTE:** If you have more than one masternode then your output list will be larger. In this case you must be sure to find the correct `txhash` and `outputidx` that belongs to your newly created masternode. The easiest way to do this is to cross-reference the output values with those that you have inputted into the `masternode.conf` file until you find an entry that hasn't been added yet.
- You will need the `txhash` and `outputidx` values for the next step, so once you have found the correct values, you can continue below

**2) Update the masternode.conf File:**
- Open the `masternode.conf` file in a text-editor app such as notepad on Windows. The easiest way to do this is from the main menu (Tools > Open Masternode Configuration File). If this does not open the file for editing, you will have to locate the file manually and open it. Windows PCs can find the file installed to the `%appdata%\Exor` directory by default.
- A single line must be added to the `masternode.conf` file for each Exor masternode. The line must be inserted at the bottom of the file on a new line. Enter the line in this format: `wallet_alias vps_ip_address:masternode_port genkey_value txhash outputidx`
- Here is a sample of what the line should look like with values filled in: `MN1 [2001:19f0:b001:d42:5123::1]:51572 48rot6GkvTTFyQAv5ztM72cYJ3uYbo6gjDb9msXiKFPd84Q5R6F 813656a0465ce757f7a02c03c0e4a6174f6d7c4c021d920d4a47cbdaa1045c02 0`
- `MN1` is the `wallet_alias` value from the `Pre-Setup Local Wallet` step
- `[2001:19f0:b001:d42:5123::1]` is the public IPv6 address that the masternode responds to and is provided at the end of the masternode install script
- `51572` is the Exor masternode port # and it must never be set to anything else
- `48rot6GkvTTFyQAv5ztM72cYJ3uYbo6gjDb9msXiKFPd84Q5R6F` is the genkey value which is generated during the Linux masternode wallet install and provided at the end of the masternode install script
- `813656a0465ce757f7a02c03c0e4a6174f6d7c4c021d920d4a47cbdaa1045c02` is the txhash value from the `getmasternodeoutputs` cmd
- `0` is the outputidx value from the `getmasternodeoutputs` cmd
- Once you have entered the data correctly you must save and close the file
- For the changes to take effect, you must restart the wallet software. Exit the wallet properly and wait a minute or two for it to completely close
- Once the wallet has stopped, you may start it again

**3) Start the Masternode:**
- Click into the `Masternodes` tab
- Find your masternode in the list and click on it to highlight it
- With the new masternode highlighted, click the `Start alias` button
- Click `Yes` when it asks if you are sure you want to start the masternode
-	If your wallet is encrypted (it should be!) and it is also still locked, it will now ask you to unlock the wallet which you can do by entering your unlock passphrase. Ensure that the `For staking only` checkbox is NOT checked, and click the `OK` button
- If all went well, you should receive a msg that says: "Successfully started masternode"
- To be 100% sure that the masternode is actually started correctly, you can review the [Verify Masternode Setup (Optional)](#verify-masternode-setup-optional) steps below

## VPS Wallet Update

**NOTE:** These update instructions are for masternodes that are already set up and working but need to be updated to the newest wallet version.

**1) Update VPS Wallet:**
- Log into the VPS as the new user account created in the `Pre-Setup VPS` instructions. (`masternodeuser` in this example)
- After the masternode wallet is already installed, running the install script subsequent times is smart enough to pick up on your previous settings and therefore you only have to execute with default settings to update and refresh the wallet files. Type the following cmd and press [ENTER]: `sudo sh exor-mn-installer.sh`
- **NOTE:** The wallet update process is completely automated and can take several minutes to complete. Please be patient while your masternode wallet is configured and updated

## Verify Masternode Setup (Optional)

**1) Verify Masternode is Started:**
- Log into the VPS as the new user account created in the `Pre-Setup VPS` instructions. (`masternodeuser` in this example)
- Type the following cmd and press [ENTER]: `exor-cli getmasternodestatus`
- If the masternode is started you will see a message that says "Masternode successfully started" like this:
```
{
  "txhash": "813656a0465ce757f7a02c03c0e4a6174f6d7c4c021d920d4a47cbdaa1045c02",
  "outputidx": 0,
  "netaddr": "[2001:19f0:b001:d42:5123::1]:51572",
  "addr": "EXkTwqKxfPg6rFrtPzLBhDJTjS7vnaFBd9",
  "status": 4,
  "message": "Masternode successfully started"
}
```
- If for any reason the masternode did not start properly you will see an error message like this:
```
error: {"code":-1,"message":"Masternode not found in the list of available masternodes. Current status: Not capable masternode: Hot node, waiting for remote activation."}
```

**2) Masternode Troubleshooting:**
- If you followed all the steps and still cannot get the masternode to start, here are some helpful hints:
- Stop and restart both the local and VPS wallets and restart the masternode again using the `Start alias` button
- Check your VPS masternode wallet block count versus the block explorer to ensure that you are fully synced to the correct block. To check the current block count of your VPS wallet type the following cmd and press [ENTER]: `exor-cli getblockcount`
- If you set up your masternode in less than 15 minutes it may require you to wait longer before activation is successful. Try restarting the wallet again using the `Start alias` button after waiting an additional 15 minutes
- If all else fails you can visit https://exor.io for support options
