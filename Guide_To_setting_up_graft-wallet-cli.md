# Setting up a working graft-wallet-cli

**Note it is recommended to not use the *root* user when working with Linux machines, especially if it concerns financial applications**

*Please consult the below guides first section on how to setup a non-root user. This is not essential.

Getting ready:

Any Ubuntu 18.04 machine is sufficient. Ubuntu server or Ubuntu Desktop will work just fine.
Secondly you need the details required for staking which you can get from your Supernode, if you are having your Supernode hosted then the URL would be provided to you, if will look something like below:

http://122.78.15.123:18690/dapi/v2.0/cryptonode/getwalletaddress

You can hit this from a web browser and it will return something similar to the below (below has been formatted to fit on the page, usually is in the same line), values have been changed for privacy reasons:

{"testnet":false,"wallet_public_address":"GCdAJoNiAtE9DTQ3UkdU4k3QxeUkbXrKUSECksrL3BuchT4hb1mEkfkfKcoNWdPp1icjDWk3jhjKgh2",
"id_key":"608e8d854a2eb902138b74607d1e0108a7a6c7e903095ab6d984d20c3834324f",
"signature":"e6ec3a907acf1684dcaa3b31b768ca65f0f605c246833b5141ddb1ad21321l80a8566bf01be8f58b8da7704280539d288679260c09d44a576b9234e53b2f2df01"}

Record this for use later



## Using a remote node : The quickest way, but not the most secure option, only use with a trusted remote node:

- Step 1 : Downloading the binaries and unzipping.
````
cd
curl https://github.com/graft-project/GraftNetwork/releases/download/v1.7.4/GraftNetwork_1.7.4.ubuntu-18.04-x64.tar.gz | tar xzf -
````
- Step 2 : accessing the folder you just downloaded and unzipped.
````
cd GraftNetwork_1.7.4.ubuntu-18.04-x64
````
Step 3 : I would suggest you create a seperate directory for you wallet like below:
````
mkdir -p ~/graft-wallets
````
Step 4 : copy graft-wallet-cli to the newly created directory.
````
cp ~/GraftNetwork_1.7.4.ubuntu-18.04-x64/graft-wallet-cli ~/graft-wallets/graft-wallet-cli
````
now go to that directory by using "cd"
````
cd ~/graft-wallets
````
## Creating a New Wallet and connecting to the remote node. If you have completed the section where you are hosting graftnoded yourself, please remove the "--daemon-host graft.community" part in the command.

***Ensure you are in the directory where graft-wallet-cli exists*** To check the contents of a drectory use "ls"

````
./graft-wallet-cli --daemon-host graft.community
````
- Step 3b : Restoring an existing wallet
````
./graft-wallet-cli --daemon-host graft.community --restore-deterministic-wallet
````
- Follow the prompts

# NB NB NB Remember to back up your SEED and keep it safe. just type seed in the wallet and follow the prompts.

FOr extra commands available just telp help and press enter inside the wallet.

## Staking your Supernode

- Step 1 : Firstly ensure your wallet is reflecting the funds needed once you have you wallet open:
````
balance
````
- Step 2 : If your funds are present, use the details you recorded earlier from the response from your SN:
````
stake_transfer <wallet_public_address> <STAKE_AMOUNT> <LOCK_BLOCKS_COUNT> <id_key> <signature>
````

So for the example used in the beginning of the guide, the staking command would look like the below to stake a T1 for 100 blocks = approx 3 hours 20 minutes, (PLEASE DO NOT USE THE BELOW IT IS JUST AN EXAMPLE):
````
stake_transfer GCdAJoNiAtE9DTQ3UkdU4k3QxeUkbXrKUSECksrL3BuchT4hb1mEkfkfKcoNWdPp1icjDWk3jhjKgh2 50000 100 608e8d854a2eb902138b74607d1e0108a7a6c7e903095ab6d984d20c3834324f e6ec3a907acf1684dcaa3b31b768ca65f0f605c246833b5141ddb1ad21321l80a8566bf01be8f58b8da7704280539d288679260c09d44a576b9234e53b2f2df01
````

Note the current maximum LOCK_BLOCKS_COUNT is equal to 5000, this is approximately a week. make sure you know what you are doing before actioning this number.

## Hosting your own graftnoded and blockchain = The slower but more secure way:

- Step 1 : Downloading the binaries and unzipping.
````
cd
curl https://github.com/graft-project/GraftNetwork/releases/download/v1.7.4/GraftNetwork_1.7.4.ubuntu-18.04-x64.tar.gz | tar xzf -
````
- Step 2 : accessing the folder you just downloaded and unzipped.
````
cd GraftNetwork_1.7.4.ubuntu-18.04-x64
````
Step 3a : Starting graftnoded and partially syncing the blockhain, in order to download the blockchain from a trusted source
````
./graftnoded
````
Leave it for a couple minutes until you see it has synced some blocks. Stop graftnoded by using either "cntrl + c" or typing exit.

- Step 3ab : Now you can download the blockchain to speed up the process. Perform the below commands in order, wait for the curl command to complete as it is the download portion:
````
cd $HOME/.graft/
ls -la
rm -r lmdb
curl http://graftbuilds-ohio.s3.amazonaws.com/lmdb.tar.gz | tar xzf -
cd lmdb && rm em* && rm lo*
cd ~/GraftNetwork_1.7.4.ubuntu-18.04-x64
````
Now you can start graftnoded again but this time we will luanch it in detached mode so it runs in the background
````
./graftnoded --detach
````
- Step 3b : If you dont want to download the blockchain just run the above command "./graftnoded --detach" from the "~/GraftNetwork_1.7.4.ubuntu-18.04-x64" directory and let it sync fully.
- Step 4 : Checking graftnoded's status.
````
./graftnoded status
````

Once fully synced you can return to the Creating a New Wallet section and complete that and the staking your supernode section.

Note for further commands for graftnoded you can run some thing like the below.
````
./graftnoded help
````
For launch flags for graftnoded
````
./graftnoded --help
````
Written by:

#### Telegram Handle: @Fezz27

# USEFUL LINKS & RESOURCES

- [Graft Community Guide to deploying a Supernode](https://github.com/graft-community/docs/blob/master/MaintenanceandServerHardeningguideforGraftSupernode_MultiSN_CommunityDebpackages.md)

- [Graft Community Deb packages, systemd and logrotate setup and Basic Server hardening steps, including operating graftnoded and graft-wallet-cli, troubleshooting supernode and using TMUX](https://github.com/graft-community/docs/blob/master/Graft_Supernode_Mainnet_Simple-step-by-step-setup-instructions-for-non-Linux-users_v1.7.md)