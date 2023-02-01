# Testnet installation guide

## General remarks
The testnet is generated with much of the settings of the orginal mainnet. The important thing is that it is going to be developed on the new repository, which is different to the current one.
Guide is rather detailed and intended also for unexperienced users.

## Prerequisites

### Setup go environment

Add a line to Your *.profile* file

    export PATH=$PATH:<path_to_go_bin_directory>

Reload profile

    . .profile

### Clone repo

Download the new repo

    git clone https://github.com/cryptonetD/lumenx.git

Checkout the dev repository

    git checkout dev

### Install binary

Compile binary

    cd lumenx
    make install

### Copy binary to local bin

    sudo su -
    cd /usr/local/bin
    cp /home/<user_dir>/go/bin/lumenxd .

## Init testnet environment

Prepare Your node to work with testnet.

    lumenxd init "<YOUR NODE MONIKER>" --chain-id lumenx-test

Pay special attention to the *chain-id*. It has to be exactly as value above.

## Download genesis.json

This step is important and have to be repeated after the final version of genesis is going to be ready. Intention is to start chain with all genesis validators present in the file, so it has to be distributed among all participants.

First version of *genesis.json* is lacking accounts and gentx, but has all other blocks needed to start the chain and can be reviewed.

You can download initial genesis.json from here: [genesis.json](./genesis.json)

## Prepare account

Idea is to start the chain with one account per each of the participants. Prepare Your new account for lumenx testnet

    lumenxd keys add <your_key_name> --keyring-backend file

Remember to secure the response You get on Your terminal.

### Pass the account to the project

Your account address is going to be needed to create correct genesis with certain amount of lumen in it. So You have to place it here:

## Prepare gentx file

This part is the most complicated one and has to be done with care. Gentx transactions will create genesis validators records in the zero block of the chain.
Gentx transaction has to be created with the key created above. Chain has to be correctly initiated. Amount of initial transaction is also important. **You can not create transaction with lower amount!**

    ./lumenxd gentx <YOUR_KEY_NAME> 1000000ulumen --generate-only --offline --chain-id lumenx-test --amount 1000000stake

The output of the command is going to be file located in .lumenx/config/gentx/ folder. **The content of the file can not be changed manually!**. Place the content of the file here:

## Modify the persistent_peers

The chain has to communicate. The initial peers list consists of two addresses. It is enough to start chain: [persistent_peers](./persistent_peers.txt)

