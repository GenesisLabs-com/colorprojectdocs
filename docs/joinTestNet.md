## Join `color-testnet` Public Testnet

`color-test-1001` is now live!


The genesis files are in [./latest](latest)

We have airdropped testnet tokens to all participants in the last Cosmos Hub
testnet. If you would like to participate and have not received tokens, you can get some from this faucet.

## How to Run a Testnet Validator

Please refer to the Color Hub documentation on validators for a general overview of running a validator. We are using the exact same validator model and software, but with slightly different parameters and other functionality specific to Color Network.


### Prerequisites
```
$ sudo apt-get install gcc g++
```
### Install GO
```
$ wget https://raw.githubusercontent.com/jim380/node_tooling/master/Cosmos/CLI/go_install.sh
$ chmod +x go_install.sh
$ ./go_install.sh -v 1.12.5
```
At the time of this writing, `1.12.5` is the latest version of Golang. **Go 1.12+ is required for the Color SDK.**
### Install COLOR
```
$ mkdir -p $GOPATH/src/github.com/color
$ cd $GOPATH/src/github.com/color
$ git clone -b <latest-release-tag> https://github.com/color-network/color-ledger
$ cd color-ledger
$ make install
```
Find the latest release tags [here](https://github.com/color-network/color-ledger/releases). To verify if installation was successful:
```
$ colord version --long
$ colorcli version --long
```
### Setting Up a New Node
```
$ colord init --chain-id=color-test-1001 <your_moniker>
$ colorcli keys add <your_wallet_name>

##
```
**Make sure you back up the mnemonics !!!**

### Creating a Validator
*If you are joining at genesis scroll down to the section on Creating a Genesis Validator!*

Please follow the documentation provided on [creating a validator for Cosmos hub](https://github.com/cosmos/gaia/blob/master/docs/validators/validator-setup.md#create-your-validator), replacing `gaiad` and `gaiacli` with `colord` and `colorcli` respectively. Also our testnet staking token denomination is `color` and Color addresses begin with `color:` instead of `cosmos`.

### Creating a Genesis Validator

*This section applies ONLY if you are joining at genesis! Genesis for Color Test-1001 was in June 2019*
#### Generate Genesis Transaction (optional)
```
$ colord add-genesis-account $(colorcli keys show <your_wallet_name> -a) 1000000color,1000000validatortoken
$ colord gentx --name <your_wallet_name> --amount 1000000color
```
If all goes well, you will see the following message:
```
Genesis transaction written to "/home/user/.colord/config/gentx/gentx-f8038a89034kl987ebd493b85a125624d5f4770.json"
```
#### Submit Gentx (optional)
Submit your gentx in a PR [here](https://github.com/color-network/testnets) 
### Genesis & Seeds
Fetch `genesis.json` into `colord`'s `config` directory.
```
$ curl https://raw.githubusercontent.com/color-network/testnets/master/color-test-1001/genesis.json > $HOME/.colord/config/genesis.json
```
Add seed nodes in `config.toml`.
```
$ nano $HOME/.colord/config/config.toml
```
Find the following section and add the seed nodes.
```
# Comma separated list of seed nodes to connect to
seeds = ""
```
### Make `colord` a System Service (optional)
```
$ sudo nano /lib/systemd/system/colord.service
```
Paste in the following:
```
[Unit]
Description=Color Xrnd
After=network-online.target

[Service]
User=<your_user>
ExecStart=/home/<your_user>/go_workspace/bin/colord start
StandardOutput=file:/var/log/colord/colord.log
StandardError=file:/var/log/colord/colord_error.log
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```
**This tutorial assumes `$HOME/go_workspace` to be your Go workspace. Your actual workspace directory may vary.**
#### Start Node
**Method 1** - With `systemd`
```
$ sudo systemctl enable colord
$ sudo systemctl start colord
```
Check node status
```
$ colorcli status
```
Check logs
```
$ sudo journalctl -u colord -f
```
**Method 2** - Without `systemd`
```
$ colord start
```
Check node status
```
$ colorcli status
```


## Create Validator 

To create validator you must have colors coin in you account then perform the following transaction


To get the public key for create validator the the following commnad 

```
colord tendermint show-validator

```
Now run the following command 

```
colorcli tx staking create-validator --amount=1000000tree --pubkey=xrn:valconspub1zcjduepqay6ynhexeev7me09gfx5dhr82yp4furnyyrvyhff42dl5j04f7lsrzvpn2 --moniker=rnssol-moniker --chain-id=regen-test-1001 --commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1" --gas="auto" --gas-prices="0.2color" --fees=2color --from=rnssol

```

After this the validator will be shown on your blockchain explorer.


