## Wallet

First of all, we need to create a wallet and an account for launching first contract.

* ```cleos wallet create``` (this command will create a wallet and return a password. You need to save it.)
* ```cleos create key``` (this command will create a keypair)

You need to create and store two sets of keypairs for your future account. One of them will be "Owner key". Another will be "Active key".

There is example below.

```
$ cleos create key
Private key: 5JgcuRV6H4eJ4UWV6RN8NnvAeWrjh8DzXBtRKxauKy6tXYD7FUu
Public key: EOS5NEMYbtjMMmcivWsXm7sJ8mo3myjRaFDrUgk8D1YQwfJ5uvNhy
```

* ```cleos wallet import <PRIVATE KEY> ```

You need run this command for each private key from stored keypairs.

* ```cleos wallet keys```

You can check that your wallet has your created keys. Make sure there are 3 private keys in your wallet. On of them will be from config.ini.

## Loading the Bios Contract

Now that we have a wallet with the key for the eosio account loaded, we can set a default system contract. For the purposes of development, the default eosio.bios contract can be used. 

```
$ cd ~/eos/build/contracts
$ cleos set contract eosio eosio.bios -p eosio
```
## Creating an account "hello" for contract

```
$ cleos create account eosio hello <Owner Public Key> <Active Public Key> 
```

## Source code of the contract "hello" (hello.cpp)

First of all, we need to create a directory "hello".
```
$ cd
$ mkdir hello
$ cd hello
```

Then we need to create **hello.cpp** file.

```
#include <eosiolib/eosio.hpp>

using namespace eosio;

class hello : public contract {

	using contract::contract;

public:

	void hi(account_name user) {
		print( "hello, ", name{user});
	}
};

EOSIO_ABI(hello, (hi))
```

## Compiling a contract

```
$ eosiocpp -o hello.wast hello.cpp
$ eosiocpp -g hello.abi hello.cpp
```

## Publishing a contract

```
$ cd ..
$ cleos set contract hello hello -p hello
```
Option **-p** means that we publish contract using hello account's permissions.

You will get output like as:

```
Reading WAST/WASM from hello/hello.wasm...
Using already assembled WASM...
Publishing contract...
executed transaction: 74cb1dfb3388840dac812a2b2c712acfeeb29164540c2bb39b34c2eb99e978a7  1792 bytes  1232 us
#         eosio <= eosio::setcode               {"account":"hello","vmtype":0,"vmversion":0,"code":"0061736d01000000013b0c60027f7e006000017e60027e7e...
#         eosio <= eosio::setabi                {"account":"hello","abi":"00010c6163636f756e745f6e616d650675696e74363401026869000104757365720c616363...
```

## Testing a contract

```
$ cleos push action hello hi '["world"]' -p eosio
```
You will have output like as

```
executed transaction: 28d92256c8ffd8b0255be324e4596b7c745f50f85722d0c4400471bc184b9a16  244 bytes  1000 cycles
#    hello <= hello::hi               {"user":"world"}
>> hello, user
```
