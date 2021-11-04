# Zenon-Testnet Node Tips

## I created this guide to help Zenon testnet users diagnose and fix errors in their nodes. The tips below are my own and not endorsed by the Zenon team. Please use the issue feature for fixes here. If you need to reach me 1:1 DM me on TG @SultanOfStaking - Try at your own risk.

## How to diagnose errors in your node:

A good first step when diagnosing your node is to pull logs. To do this open a terminal window logged into your node and run the following command 

`sudo tail -f /var/log/syslog`

OR

`sudo journalctl -f -u go-zenon`

You will either see logs of block production with no errors (that's good), block production with errors (these may be nothing or may need to be fixed - when in doubt ask the community), or your node may not be running. Assuming simply looking at the logs didn't give you enough info you can stop and start your node and watch the start up process for errors. To do this open another terminal window, log into your node, and run

`./znn-controller`

Then 

`4` to stop your service

Then

`3` to start the service 

The first window you opened should now show logs of the startup process then block production. If you get errors in the startup process you need to fix them. I have compiled a list of common errors below. Please submit an issue or message me if you encounter an error I need to add to the list.

### To stop the logs at any time press control+c

## Common errors:
A note to start, if you set up a new user for security purposes and downloaded the bundle there be aware that znnd will use the "root" path so you may need to move files from your new user to root or change base path in the service file. An easier way is to install the node from root from the start.

- **Config file missing:** Ensure config is in correct path "/root/.znn/config.json". The contents of this file should mirror those on the [Zenon pillar deployment page](https://testnet.znn.space/#!deploy.md)

- **Config malformed**: To troubleshoot this stop the znnd.service, disableRPC, then enableRPC. When you enableRPC you should get a readout telling you the line and character that is throwing the error in your config.json. Once you have that information edit the config file accordingly to fix the error. You will know the error is fixed when you can disableRPC and get "successful" return, then enableRPC and get "successful" return. For help with any of the commands mentioned refer to the [Zenon CLI Guide](https://testnet.znn.space/#!cli.md)

- **znnd is not detecting my wallet**: Ensure yourn wallet is in the correct path "/root/.znn/wallet". In this example running `cd wallet` from "/root/.znn" should show your producer address and syrius address.

- **Unavailable resources (no disk space)**: Likely in logs but to chase down what is taking up the space run `du -cha --max-depth=1 / | grep -E "M|G"` This will tell you what directory is taking up the space. From there you can drill down until you find the culprit. For example, if "root" is using all your memory run `du -cha --max-depth=1 /root | grep -E "M|G"` then if .zenon is the suspect `du -cha --max-depth=1 /root/.zenon | grep -E "M|G"` and so on. 

## Other commands worth knowing

### All Zenon CLI commands can be found here https://testnet.znn.space/#!cli.md

### How to edit config.json

`nano ~/.znn/config.json`

### How to pull logs:

`sudo tail -f /var/log/syslog`

### How to move files

`sudo mv yourfile ./yourpath/yourpath/yourfile` 

This is helpful if you get errors indicating zenon cannot find data, wallet, etc. When you start your node logs should show you the paths being used for data and config. If your files on your machine are not in that path you need to move them. For example, if config path on startup is (making this up) root/znnd/config and your path is user1/znnd/config you would need to navigate to user1/znnd and run “sudo mv config ./root/znnd/config"

### Some Basic Linux Commands: https://linuxcommand.org/lc3_man_pages/ssh1.html 
    * Escape (END) = press q to get back to cli
    * Save = CTRL+O
    * Exit = CTRL+X
    * Remove = rm 
    * Remove files in directory = rm -r
    * Remove directory = rm -rf 
    * Terminate program = ctrl+c
    * View free memory = free -m
    * View memory usage = ps aux
    * View logs = 
        * cd /var/log 
        * ls
        * sudo nano /var/log/log you want to view
        * sudo tail -f /var/log/syslog
    * Exit log = ctrl+c

## If you found helpful no need to donate, but delegation to SultanOfStaking pillar would be appreciated https://explorer.znn.space/pillar/z1qpgdtn89u9365jr7ltdxu29fy52pnzwe4fl7zc
