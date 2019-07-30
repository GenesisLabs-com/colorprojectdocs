# Ledger Nano integration 

Ledger Nano is a hardware device that allows user to send and recieve transaction using this hardware wallet.
To work with color testnet ledgner nano should be connected with light client to process transactions.

## Requirments 

 
 - [Install ledger live](https://shop.ledger.com/pages/ledger-live)
 - [Install ledger Nano](https://shop.ledger.com/products/ledger-nano-s)

## Backend Setup

After installing `ledger nano live` , if it is your new device initialize it as a new device and follow the 5 steps in ledger live menu .

####  if the process get stuck at :
- connect and unlock your ledger device
- navigate through your dashboard

 follow these commands to configure leger with ledger live 
###  1. setup
- Check if the plugdev group exists by entering the command:

```bash
cat /etc/group | grep plugdev
```
- Check if the plugdev group exists by entering the command:
```bash
cat /etc/group | grep plugdev
```
##### Follow the steps below if the previous command did not return a result
- Create the plugdev group:
```bash
 sudo groupadd plugdev
```
- Check if you are in the group plugdev with the command:
```bash
 group
```
-  If the output does not contain plugdev, you are not in the plugdev group. Enter the command:
```bash
 sudo gpasswd -a <user> plugdev
```
##### `Note: replace <user> by your username, e.g for user "mike", it would be: sudo gpasswd -a mike plugdev.` 

-  Logout and login for the change to take effect. To verify you are now in the plugdev group, enter:
```bash
  groups
```
- and search for a plugdev occurrence. If it's not there, you've missed a step and should restart from step 1.

###  2. Add the udev rules

- Enter the following command to automatically add the rules and reload udev:

```bash
    wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```
- Retry connecting your Ledger Nano S with Ledger Live.

##### If it's still not working, continue to step 3 

### 3.Troubleshooting.

`Try each of the following three options.` 

#### Option 1
- Edit the file `/etc/udev/rules.d/20-hw1.rules`file by adding the `OWNER="<user>"` parameter to each line, where `<user>` is your Linux user name.
    Then reload the rules as follows:
```bash
    udevadm trigger
    udevadm control --reload-rules
```
-  Retry the connection with Ledger Live. If it does not work, try the next option.
#### Option 2
- Edit the `/etc/udev/rules.d/20-hw1.rules` file and add the following lines:
```bash
    KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660", GROUP="plugdev", ATTRS{idVendor}=="2c97"

    KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660", GROUP="plugdev", ATTRS{idVendor}=="2581"
```
   -  Then reload the rules:
```bash
    udevadm trigger
    udevadm control --reload-rules
```
- Retry connecting with Ledger Live. If it does not work yet, try the last option.
#### Option 3
- If you are on Arch Linux, you can try the following rules:
```bash
    /etc/udev/rules.d/20-hw1.rules
```
```bash
    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="1b7c", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="2b7c", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="3b7c", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="4b7c", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="1807", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2581", ATTRS{idProduct}=="1808", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2c97", ATTRS{idProduct}=="0000", MODE="0660", TAG+="uaccess", TAG+="udev-acl"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2c97", ATTRS{idProduct}=="0001", MODE="0660", TAG+="uaccess", TAG+="udev-acl”

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="2c97", ATTRS{idProduct}=="0004", MODE="0660", TAG+="uaccess", TAG+="udev-acl"
```
   -  Then reload the rules again and retry the connection with Ledger Live:
```bash
    udevadm trigger
    udevadm control --reload-rules
```

Still not working after following all the steps correctly? Please contact 
[ledger support](https://support.ledger.com/hc/en-us/requests/new).

## `After that` 
Allow ledger manager on your device by pressing right button on ledger.
This will open ledger live dashborad for you 
### Install Cosmos App
`Follow these steps to install cosoms app on your device.`
- unlock your device by providing pin 
 - go to settings on top right corner an select `Experimental Features `
- check on ` developer mode`
- got to `manager` in ledger live left navigation bar search for `cosmos`
- install updated version by clicking `install button `
- this will install cosoms app on your ledger device 

## Frontend  Setup
Connect your colors with wallet server, and follow these steps 
#### Sign in using ledger nano:
 `To sign in using ledger nano you will have to:`

-  unlock ledger device by entering pin code 
-  open cosmos app in ledger device else you cannnot sign in .
-  After sucessfull signin you will see your dashbord 
- open `wallet`in left naviagtion bar in wallet server 
- fill out the form for transaction 
 `sender address` `amount`and 
  `gas`          
- sign the transaction using ledger device by pressing right button on ledger on `sign transaction `option.
- you have sucessfully sent a valid transaction.

        
