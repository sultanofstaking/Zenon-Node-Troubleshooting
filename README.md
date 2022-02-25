# Zenon Pillar Troubleshooting

## I created this guide to help Zenon users diagnose and fix errors in their nodes. The tips below are my own and not endorsed by the Zenon team. Please use the issue feature for fixes here. If you need to reach me 1:1 DM me on TG @SultanOfStaking - Try at your own risk.

## How to diagnose errors in your node:

A good first step when diagnosing your node is to pull logs. I recommend you open a terminal to issue commands and another to monitor logs. Log into your node and run the following command on one of your machines

`sudo tail -f /var/log/syslog` OR `sudo journalctl -f -u go-zenon`

In logs you will see active block production with no errors (that's good), block production with errors (these may be nothing or may need to be fixed - when in doubt ask the community), or your node may not be running. 

To stop logs press ctrl+c at any time. The commands above will show a truncated version of logs which is good as any errors most likely happened near the end of the logs. If you want to see full logs enter `sudo less /var/log/syslog`. To exit these logs press q

If your node is not producing blocks and shows no errors then usually simply restarting the node will solve your problem. To do this enter the following command in the screen not running logs `./znn-controller` then select the option to stop your service and start your service OR try running `sudo systemctl restart go-zenon.service`

In the screen running logs you should see messages reflecting your node has stopped, started, then hopefully block production begins.

If you get errors in the startup process you need to fix them. I have compiled a list of common errors below. Please submit an issue or message me if you encounter an error I need to add to the list.

## Common errors:
A note to start, if you set up a new user for security purposes and downloaded the bundle there be aware that znnd will use the "root" path so you may need to move files from your new user to root or change base path in the service file. An easier way is to install the node from root from the start.

- **My wallet is not detected**: Ensure yourn wallet is in the correct path "/root/.znn/wallet". In this example running `ls - lha` from "/root/.znn/wallet" should show your producer address. If not, check that same path in the user you created and downloaded the bundle with then move the wallet (move steps are further down). 

- **Config file missing:** Ensure config is in correct path "/root/.znn/config.json". The contents of this file should mirror those on the [Zenon pillar deployment page](https://testnet.znn.space/#!deploy.md)

- **Config malformed**: To troubleshoot this stop the znnd.service, disableRPC, then enableRPC. When you enableRPC you should get a readout telling you the line and character that is throwing the error in your config.json. Once you have that information edit the config file accordingly to fix the error. You will know the error is fixed when you can disableRPC and get "successful" return, then enableRPC and get "successful" return. For help with any of the commands mentioned refer to the [Zenon CLI Guide](https://testnet.znn.space/#!cli.md)

- **Unavailable resources (no disk space)**: To chase down what is taking up the space run `du -cha --max-depth=1 / | grep -E "M|G"` This will tell you what directory is taking up the space. From there you can drill down until you find the culprit. For example, if "root" is using all your memory run `du -cha --max-depth=1 /root | grep -E "M|G"` then if .zenon is the suspect `du -cha --max-depth=1 /root/.zenon | grep -E "M|G"` and so on. 

### If you are getting errors not listed here please submit an issue and I will update this guide.

## Commands Worth Knowing

Get all available commands

`./znn-cli --help`

Check wallet version

`./znn-cli version`

Check height & hash

`./znn-cli frontierMomentum`

go-zenon service commands

```
sudo systemctl stop go-zenon.service
sudo systemctl daemon-reload
sudo systemctl start go-zenon.service
sudo systemctl restart go-zenon.service
systemctl status go-zenon.service
```

Edit config.json

`sudo nano ~/.znn/config.json`

Edit service file

`sudo nano /etc/systemd/system/go-zenon.service`

Move files

`sudo mv yourfile ./yourpath/yourpath/yourfile` 

This is helpful if you get errors indicating zenon cannot find data, wallet, etc. When you start your node logs should show you the paths being used for data and config. If your files on your machine are not in that path you need to move them. For example, if config path on startup is (making this up) root/znnd/config and your path is user1/znnd/config you would need to navigate to user1/znnd and run “sudo mv config ./root/znnd/config"

### Basic Linux Commands: https://linuxcommand.org/lc3_man_pages/ssh1.html 
Escape (END) = press q to get back to cli
- Save = CTRL+O
- Exit = CTRL+X
- Remove = `rm`
- Remove files in directory = `rm -r`
- Remove directory = `rm -rf`
- Terminate program = `ctrl+c`
- View free memory = `free -m`
- View memory usage = `ps aux`
- View logs = 
`cd /var/log`
`ls`
`sudo nano /var/log/log` replace log with what you want to view
- Exit log = ctrl+c

### System utilities to view performance
- Memory usage with `vmstat` `top` `free` `htop`
- CPU/memory/disk with `sar`
- Bandwidth with `iftop`
- Network protocols with `ntopng`

### Memory issues
To increase CPU you will need to upgrade your machine. To upgrade disk space you can add a volume then log into your machine and run `df -h` to identify the filesystem path then run `sudo resize2fs YOUR PATH` replacing YOUR PATH with the path displayed under filesystem in the previous step. 

## If you found helpful no need to donate, but delegation to SultanOfStaking pillar would be appreciated!
