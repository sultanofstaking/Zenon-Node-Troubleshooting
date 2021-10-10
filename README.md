# Zenon-Testnet Node Tips

## I created this guide to help Zenon testnet users diagnose and fix errors in their nodes. The tips below are my own and not endorsed by the Zenon team. Try at your own risk.

## How to diagnose errors in your node:

A good first step when diagnosing your node is to pull logs. To do this open a terminal window logged into your node and run the following command 

`sudo journalctl -f -u znnd.service`

You will either see logs of block production with no errors (that's good), block production with errors (these may be nothing or may need to be fixed - when in doubt ask the community), or your node may not be running. Assuming simply looking at the logs didn't give you enough info you can stop and start your node and watch the start up process for errors. To do this open another terminal window, log into your node, and run

`sudo systemctl stop znnd.service`

Then 

`sudo systemctl start znnd.service`

The first window you opened should now show logs of the startup process then block production. If you get errors in the startup process you need to fix them. I have compiled a list of common errors below. Please submit an issue or message me if you encounter an error I need to add to the list.

## Common errors:
A note to start, if you set up a new user for security be aware that znnd will use the "root" path so you may need to move files from your new user to root or change base path in the service file.

- **Config file missing:** Ensure config is in correct path "/root/.znn/config.json". The contents of this file should mirror those on the [Zenon pillar deployment page](https://testnet.znn.space/#!deploy.md)

- **Config malformed**: To troubleshoot this stop the znnd.service, disableRPC, then enableRPC. When you enableRPC you should get a readout telling you the line and character that is throwing the error in your config.json. Once you have that information edit the config file accordingly to fix the error. You will know the error is fixed when you can disableRPC and get "successful" return, then enableRPC and get "successful" return. For help with any of the commands mentioned refer to the [Zenon CLI Guide](x.com)

- **znnd is not detecting my wallet**: Ensure yourn wallet is in the correct path "/root/.znn/wallet". In this example running `cd wallet` from "/root/.znn" should show your producer address and syrius address.

## Other commands worth knowing
### How to edit config.json

`nano ~/.znn/config.json`

### How to edit znnd.service (note you will need to reload the daemon for changes to go into effect)

`sudo nano /etc/systemd/system/znnd.service`

### Common znnd.service commands:
`````
sudo systemctl stop znnd.service
sudo systemctl daemon-reload
sudo systemctl start znnd.service
sudo systemctl restart znnd.service
systemctl status znnd.service
`````
### How to pull logs:

`sudo journalctl -f -u znnd.service`

or

`sudo tail -f /var/log/syslog`

## If you found helpful no need to donate, but delegation to SultanOfStaking pillar would be appreciated https://explorer.znn.space/pillar/z1qpgdtn89u9365jr7ltdxu29fy52pnzwe4fl7zc
