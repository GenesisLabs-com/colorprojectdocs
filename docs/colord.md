## Single-node, Local, Manual Testnet

This guide helps you create a single validator node that runs a network locally for testing and other development related uses.

### Requirements

- [Install Color](./index.md)
- [Install `jq`](https://stedolan.github.io/jq/download/) (optional)

### Create Genesis File and Start the Network

```bash
# You can run all of these commands from your home directory
cd $HOME

# Initialize the genesis.json file that will help you to bootstrap the network
colord init <moniker name> --chain-id=colorchain

# Create a key to hold your validator account
colorcli keys add validator

# Add that key into the genesis.app_state.accounts array in the genesis file
# NOTE: this command lets you set the number of coins. Make sure this account has some coins
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
colord add-genesis-account $(colorcli keys show validator -a) 1000000000stake

# Generate the transaction that creates your validator
colord gentx --name validator

# Add the generated bonding transaction to the genesis file
colord collect-gentxs

# Now its safe to start `colord`
colord start
```

This setup puts all the data for `colord` in `~/.colord`. You can examine the genesis file you created at `~/.colord/config/genesis.json`. With this configuration `colorcli` is also ready to use and has an account with tokens (both staking and custom).


## Multi-node, Local, Automated Testnet

From the [networks/local directory](https://github.com/cosmos/gaia/tree/master/networks/local):

### Requirements

- [Install color](./index.md)
- [Install docker](https://docs.docker.com/engine/installation/)
- [Install docker-compose](https://docs.docker.com/compose/install/)

### Build

Build the `colord` binary (linux) and the `tendermint/colornode` docker image required for running the `localnet` commands. This binary will be mounted into the container and can be updated rebuilding the image, so you only need to build the image once.

```bash
# Work from the SDK repo
cd $GOPATH/src/github.com/cosmos/gaia

# Build the linux binary in ./build
make build-linux

# Build tendermint/colordnode image
make build-docker-colordnode
```

### Run Your Testnet

To start a 4 node testnet run:

```
make localnet-start
```

This command creates a 4-node network using the colornode image.
The ports for each node are found in this table:

| Node ID | P2P Port | RPC Port |
| --------|-------|------|
| `colordnode0` | `26656` | `26657` |
| `colordnode1` | `26659` | `26660` |
| `colordnode2` | `26661` | `26662` |
| `colordnode3` | `26663` | `26664` |

To update the binary, just rebuild it and restart the nodes:

```
make build-linux localnet-start
```

### Configuration

The `make localnet-start` creates files for a 4-node testnet in `./build` by
calling the `colord testnet` command. This outputs a handful of files in the
`./build` directory:

```bash
$ tree -L 2 build/
build/
├── colorcli
├── colord
├── gentxs
│   ├── node0.json
│   ├── node1.json
│   ├── node2.json
│   └── node3.json
├── node0
│   ├── colorcli
│   │   ├── key_seed.json
│   │   └── keys
│   └── colord
│       ├── ${LOG:-colord.log}
│       ├── config
│       └── data
├── node1
│   ├── colorcli
│   │   └── key_seed.json
│   └── colord
│       ├── ${LOG:-colord.log}
│       ├── config
│       └── data
├── node2
│   ├── colorcli
│   │   └── key_seed.json
│   └── colord
│       ├── ${LOG:-colord.log}
│       ├── config
│       └── data
└── node3
    ├── colorcli
    │   └── key_seed.json
    └── colord
        ├── ${LOG:-colord.log}
        ├── config
        └── data
```

Each `./build/nodeN` directory is mounted to the `/colord` directory in each container.

### Logging

Logs are saved under each `./build/nodeN/colord/color.log`. You can also watch logs
directly via Docker, for example:

```
docker logs -f colordnode0
```

### Keys & Accounts

To interact with `colorcli` and start querying state or creating txs, you use the
`colorcli` directory of any given node as your `home`, for example:

```shell
colorcli keys list --home ./build/node0/colorcli
```

Now that accounts exists, you may create new accounts and send those accounts
funds!

::: tip
**Note**: Each node's seed is located at `./build/nodeN/colorcli/key_seed.json` and can be restored to the CLI using the `colorcli keys add --restore` command
:::

### Special Binaries

If you have multiple binaries with different names, you can specify which one to run with the BINARY environment variable. The path of the binary is relative to the attached volume. For example:

```
# Run with custom binary
BINARY=colorfoo make localnet-start
```

