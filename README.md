## State of the Code

Currently working on updating the software to match [BOLT13 rev1](https://github.com/sr-gi/bolt13).

# The Eye of Satoshi (TEOS)

The Eye of Satoshi is a Lightning watchtower compliant with [BOLT13](https://github.com/sr-gi/bolt13), written in Python 3.

`teos` consists in four main modules:

- `teos`: including the tower's main functionality (server-side)
- `cli`: including a reference command line interface (client-side)
- `common`: including shared functionality between `teos` and `cli`.
- `watchtower-plugin`: including a watchtower client plugin for c-lightning.

Additionally, tests for every module can be found at `tests`.

## Dependencies
Refer to [DEPENDENCIES.md](DEPENDENCIES.md)

## Installation

Refer to [INSTALL.md](INSTALL.md)

## Running TEOS

You can run `teos` buy running `teosd.py` under `teos`:

```
python -m teos.teosd
```

`teos` comes with a default configuration that can be found at [teos/\_\_init\_\_.py](teos/__init__.py). 

The configuration includes, amongst others, where your data folder is placed, what network it connects to, etc.

To run `teos` you need a set of keys (to sign appointments) stored in your data directory. You can follow [generate keys](#generate-keys) to generate them.


### Configuration file and command line parameters

To change the configuration defaults you can:

- Define a configuration file following the template (check [teos/template.conf](teos/template.conf)) and place it in the `data_dir` (that defaults to `~/.teos/`) 

and / or 

- Add some global options when running the daemon (run `teosd.py -h` for more info).

## Running TEOS in another network

By default, `teos` runs on `mainnet`. In order to run it on another network you need to change the network parameter in the configuration file or pass the network parameter as a command line option. Notice that if teos does not find a `bitcoind` node running in the same network that it is set to run, it will refuse to run.


### Modifiying the configuration file

The configuration file options to change the network where `teos` will run are the `btc_rpc_port` and the `btc_network` under the `bitcoind` section:

```
[bitcoind]
btc_rpc_user = "user"
btc_rpc_password = "passwd"
btc_rpc_connect = "localhost"
btc_rpc_port = 8332
btc_network = "mainnet"
```

For regtest, it should look like:

```
[bitcoind]
btc_rpc_user = "user"
btc_rpc_password = "passwd"
btc_rpc_connect = "localhost"
btc_rpc_port = 18443
btc_network = "regtest"
```


### Passing command line options to `teosd`

Some configuration options can also be passed as options when running `teosd`. We can, for instance, pick the network as follows:

```
python -m teos.teosd --btcnetwork=regtest --btcrpcport=18443
```

## Interacting with a TEOS Instance

You can interact with a `teos` instance (either run by yourself or someone else) by using `teos_cli` under `cli`.

Since `teos_cli` works independently of `teos`, it uses a different configuration. The defaults can be found at [cli/\_\_init\_\_.py](cli/__init__.py). The same approach as with `teosd` is followed:

- A config file (`~/.teos_cli/teos_cli.conf`) can be set to change the defaults.
- Some options ca also be changed via command line. 
- The configuration file template can be found at [cli/template.conf](cli/template.conf))

`teos_cli` needs an independent set of keys and, top of that, a copy of tower's the public key (`teos_pk.der`). Check [generate keys](#generate-keys) for more on how to set this.

Notice that `teos_cli` is a simple way to interact with `teos`, but ideally that should be part of your wallet functionality (therefore why they are independent entities). `teos_cli` can be used as an example for how to send data to a [BOLT13](https://github.com/sr-gi/bolt13) compliant watchtower.

## Generate Keys

In order to generate a pair of keys for `teos` (or `teos_cli`) you can use `generate_keys.py`. 

The script generates and stores a set of keys on disk (by default it outputs them in the current directory and names them `teos_sk.der` and `teos_pk.der`). The name and output directory can be changed using `-n` and `-d` respectively.

The following command will generate a set of keys for `teos` and store it in the default data directory (`~/.teos`):
```
python generate_keys.py -d ~/.teos
``` 

The following command will generate a set of keys for `teos_cli` and store it in the default data directory (`~/.teos_cli`):
```
python generate_keys.py -n cli -d ~/.teos_cli
``` 

Notice that `cli` needs a copy of the tower public key, so you should make a copy of that if you're using different data directories (as in this example):

```
cp ~/.teos/teos_pk.der ~/.teos_cli/teos_pk.der 
```

## Contributing 
Refer to [CONTRIBUTING.md](CONTRIBUTING.md)