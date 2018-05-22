# Setting up of local environment

## Check System Requirements

* 8GB RAM free required
* 20GB Disk free required

## Getting the code

To download all of the code, clone the **eos** repository and its submodules.

```
cd ~
sudo apt-get install git
git clone https://github.com/EOSIO/eos --recursive
```

## Building EOSIO

### Run the build script

Run the build script from the **eos** folder.

```
cd eos
./eosio_build.sh
```

### Install the executables

For ease of contract development, content can be installed in the /usr/local folder using the make install target. This step is run from the build folder. Adequate permission is required to install, so we use the sudo command with make install.

```
cd build
sudo make install
```

## Testing EOSIO

* Find the genesis file using ```sudo find / -name genesis.json```
* It should come up with a few options, save the one that has "/eosio/nodeos/config" in it to your eostext file, for instance "/home/dmitryl/.local/share/eosio/nodeos/config/genesis.json"
* ```sudo vim ~/.local/share/eosio/nodeos/config/config.ini```
* Please, check the following options

```
# Fill this with the genesis path you save in your eostext file before.
genesis-json = /path/to/eos/source/genesis.json

# Find and switch to true
enable-stale-production = true

# Find, remove "#", and change to "eosio"
producer-name = eosio

# Copy the following plugins to the bottom of the file

# Load the block producer plugin, so you can produce blocks
plugin = eosio::producer_plugin
# Wallet plugin
plugin = eosio::wallet_api_plugin
# As well as API and HTTP plugins
plugin = eosio::chain_api_plugin
plugin = eosio::http_plugin
# This will be used by the validation step below, to view account history
plugin = eosio::history_api_plugin
```
* Run ```nodeos``` and let is keep running in it's own window.
* Run ```cleos get info``` in another terminal for checking.
