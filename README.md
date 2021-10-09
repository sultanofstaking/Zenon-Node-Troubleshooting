# Zenon-Testnet Node Tips

# I created this guide to help Zenon testnet users diagnose and fix errors in their nodes. The tips below are my own and not endorsed by the Zenon team. Try at your own risk. If you found helpful no need to donate, but delegation to SultanOfStaking pillar would be appreciated https://explorer.znn.space/pillar/z1qpgdtn89u9365jr7ltdxu29fy52pnzwe4fl7zc

# How to diagnose errors in your node:
#Open two terminal windows logged into your node. In the first window run 
sudo journalctl -f -u znnd.service
#In the second window run 
sudo systemctl stop znnd.service
#Then 
sudo systemctl start znnd.service
#The first window should show logs of the startup process then block production. If you get errors in the startup process you need to fix them.

#How to edit config.json
nano ~/.znn/config.json

#How to edit znnd.service (note you will need to reload the daemon for changes to go into effect)
sudo nano /etc/systemd/system/znnd.service

#Common znnd.service commands:
sudo systemctl stop znnd.service
sudo systemctl daemon-reload
sudo systemctl start znnd.service
sudo systemctl restart znnd.service
systemctl status znnd.service

#How to pull logs:
sudo journalctl -f -u znnd.service
#or
sudo tail -f /var/log/syslog

# Common errors:
#If you set up a new user for security be aware that znnd will use the /root/ path so you may need to move some files.

#I am getting Config file missing:
#Ensure config is in correct path "/root/.znn/config.json". The contents of this file should cover RPC and Pillar

#znnd is not detecting my wallet:
#Ensure wallet is in correct path "/root/.znn/wallet". cd wallet should show your producer address and syrius address

#I am getting Config malformed:
#You can stop the service, disableRPC, then enableRPC. When you enableRPC you should get a readout telling you the line and character in your config.json that is throwing the error. Once you have that edit the config file and fix the error. The error is fixed when you can disableRPC and get "successful" return, then enableRPC and get "successful" return.
