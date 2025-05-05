## Table of Contents
1. [> Call Move function for StarKey Wallet](#issue-summary-call-move-function-for-starkey-wallet)
2. [> Sign & Verify message using Starkey Wallet](#issue-summary-sign-a-message-using-starkey-wallet)
3. [> Error Code 0x1::resource_account: 0x60001](#issue-summary-error-code-0x1resource_account-0x60001)
4. [> Move compilation failed: supra move tool compile](#issue-summary-move-compilation-failed-supra-move-tool-compile)
5. [> Call the Modules Deployed on Supra](#issue-summary-call-the-modules-deployed-on-supra)
6. [> Digital Assets Dependency for NFT Dapp](#issue-summary-digital-assets-dependency-for-nft-dapp)
7. [> Module for Implementation on a custom coin](#issue-summary-module-for-implementation-on-a-custom-coin)
8. [> transfer function stating that account isn't registered](#issue-summary-transfer-function-stating-that-account-isnt-registered)
9. [> Calling Supra Coin](#issue-summary-calling-supra-coin)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)


NOTE: Kindly follow the below format to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Call Move function for StarKey Wallet

### ➥ SOLUTION SUMMARY:
The code snippet in here demonstrate that how can we create tx payload for entry function: https://github.com/Entropy-Foundation/supra-l1-sdk/blob/master/src/example.ts#L190

So we should convert the move function call to rawTransaction object  and pass it to the data that defined on your docs, and we ignore the sequence_number like metamask ignore the nonce and the wallet side will process that.

For raw a transaction, the process to create a `rawTx` and `serializedRawTx` is almost similar

```TypeScript
     // To send serialized transaction
  console.log(
    await supraClient.sendTxUsingSerializedRawTransaction(
      senderAccount,
      supraCoinTransferSerializedRawTransaction,
      {
        enableTransactionSimulation: true,
        enableWaitForTransaction: true,
      }
    )
  );
```
![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Sign a message using StarKey Wallet
### Couldn't find any docs regarding sign a message using Starkey Wallet, any example code that I can start with?

### ➥ SOLUTION SUMMARY:
Please update to Min V 1.1.24, This Version and Above includes the signMessage function.
Here's a code snippet for the help:

```Javascript
// Javascript
import nacl from "tweetnacl";

const hexString = '0x' + Buffer.from(signMessage, 'utf8').toString('hex')
const response = await supraProvider.signMessage({message:hexString})
console.log('signMessage response :: ', response)
    if (response) {
      const { publicKey,signature,address } = response
      const sign =remove0xPrefix(signature)
      const key =remove0xPrefix(publicKey)
      const verified = nacl.sign.detached.verify(
          new TextEncoder().encode(signMessage),
          Uint8Array.from(Buffer.from(sign, 'hex')),
          Uint8Array.from(Buffer.from(key, 'hex')),
      );
     console.log('signature :: ',signature)
     console.log('verified :: ',verified)
   }
```

```Typescript
// Typescript
const remove0xPrefix = (hexString: string) => {
  return hexString.startsWith("0x") ? hexString.slice(2) : hexString;
}
```
Demo LINK: https://frontend-web-wallet-connect-demo.vercel.app/supra-dapp

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Error Code 0x1::resource_account: 0x60001
Getting error in deploying the contract to testnet while the contracts are built without any error and error code in output is 0x1::resource_account: 0x60001

### ➥ SOLUTION SUMMARY:
This error message typically indicates a problem with resource account management within the Move code. In the context of deploying a contract, it likely means that there's an issue with how your contract interacts with the resource accounts.

The current recommendation is to use objects over resources accounts iirc: https://aptos.dev/en/build/smart-contracts/deployment
Also, Check this - https://docs.supra.com/l1/rest-api/accounts/resources#rpc-v1-accounts-address-resources

For Error code 1 of resource_account module, ECONTAINER_NOT_PUBLISHED
https://github.com/Entropy-Foundation/aptos-core/blob/b414eadb54e8e8722e58096f96dab17a11787646/aptos-move/framework/supra-framework/sources/resource_account.move#L170

It means the container resource that stores the mapping of the resource address to signer capability doesn't exist at the address you are passing

Here is a repo that I used to test the create_resource_account_and_publish_package method
https://github.com/nolan-supra/TS-SDK_Create-Resource-Account-And-Publish-Package

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Move compilation failed: supra move tool compile

Move compilation failed while doing `supra move tool compile --package-dir /supra/configs/move_workspace/PROJECT NAME`

Error message
```PowerShell
General: {
  "Error": "Move compilation failed: Unable to resolve packages for package 'hunch': While resolving dependency 'SupraFramework' in package 'hunch': Failed to reset to latest Git state 'dev' for package 'SupraFramework', to skip set --skip-fetch-latest-git-deps | Exit status: exit status: 128"
}
Error: General: {
  "Error": "Move compilation failed: Unable to resolve packages for package 'hunch': While resolving dependency 'SupraFramework' in package 'hunch': Failed to reset to latest Git state 'dev' for package 'SupraFramework', to skip set --skip-fetch-latest-git-deps | Exit status: exit status: 128"
}
```

### ➥ SOLUTION SUMMARY:
Add the dependency like this

   ```PowerShell
   SupraFramework = { git = "https://github.com/Entropy-Foundation/aptos-core.git", rev = "dev", subdir = "aptos-move/framework/supra-framework" }
   ```
![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Call the Modules Deployed on Supra
An example code or like repo to Call the Modules Deployed on Supra.

## ➥ SOLUTION SUMMARY:
All of our deployed framework modules are available in the framework folder of the github repo then api endpoint can be used to see which modules are available.

Link: https://docs.supra.com/move/rest-api/accounts/modules

   ```PowerShell
https://rpc-testnet.supra.com/rpc/v1/accounts/{address}/modules
   ```
   ```PowerShell
https://rpc-testnet.supra.com/rpc/v1/accounts/{address}/modules/{module_name}
   ```
![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Digital Assets Dependency for NFT Dapp

### ➥ SOLUTION SUMMARY:
these digital assets modules are deployed at the 0x4 address

   ```PowerShell
https://rpc-mainnet.supra.com/rpc/v1/accounts/0x4/modules
   ```
- aptos-token (easy-to-use implementation of token)

**docs:** https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/aptos-token-objects/doc/aptos_token.md

**src:** https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/aptos-token-objects/sources/aptos_token.move

- Token

**doc:** https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/aptos-token-objects/doc/token.md

**src:** https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/aptos-token-objects/sources/token.move

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Module for Implementation on a custom coin.

### ➥ SOLUTION SUMMARY:
A Move Module Made for a liquid swap on supra, Customize and use this to work for your Implementation on a custom coin:

```Move
module airdrop_deployer::bitcoin_coin {
    use std::error;
    use std::signer;
    use std::string;
    friend airdrop_deployer::airdrop;
    use supra_framework::coin;

    const ENOT_OWNER: u64 = 0;
    const E_ALREADY_HAS_CAPABILITY: u64 = 1;
    const E_DONT_HAVE_CAPABILITY: u64 = 2;

    struct BitcoinCoin {}

    struct BitcoinCapability has key {
        burn_cap: coin::BurnCapability<BitcoinCoin>,
        freeze_cap: coin::FreezeCapability<BitcoinCoin>,
        mint_cap: coin::MintCapability<BitcoinCoin>,
    }

    fun only_owner(addr: address) {
        assert!(addr == @airdrop_deployer, ENOT_OWNER);
    }


    fun init_module(account: &signer) {
        let (burn_cap, freeze_cap, mint_cap) = coin::initialize<BitcoinCoin>(
            account,
            string::utf8(b"BITCOIN"),
            string::utf8(b"BTC"),
            8,
            true,
        );
        coin::register<BitcoinCoin>(account);


        move_to(account, BitcoinCapability {
            burn_cap,
            freeze_cap,
            mint_cap,
        });
    }



    ///Can only be called by `airdrop_deployer`.
    public entry fun mint_by_owner(user: &signer, amount: u64) acquires BitcoinCapability {
        assert!(signer::address_of(user) == @airdrop_deployer, error::permission_denied(ENOT_OWNER));
        let caps = borrow_global<BitcoinCapability>(@airdrop_deployer);
        let coins = coin::mint(amount, &caps.mint_cap);
        coin::deposit(signer::address_of(user), coins);
    }
}
```

More details on what's happening here:

1. This module implements a fungible token called BitcoinCoin, represented by the symbol BTC with 8 decimal places.
2. It assigns the airdrop_deployer full control over the token's minting capabilities.
3. A BitcoinCapability resource is used to manage these privileges securely.
4. Functions include initializing the token, enforcing owner-only actions, and minting tokens to specified accounts.
5. The module ensures strict access control and leverages Move's resource model for safe and efficient token management.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Transfer function stating that account isn't registered
We are creating a token and then registering our account to have a coinstore for that token, it’s still reverting back on transfer function stating that account isn't registered.

### ➥ SOLUTION SUMMARY:
The error `account isn't registered` during the transfer function indicates that the recipient address hasn't been registered with the coin module.

Before like having token transfer and all ensure the recipient address is explicitly registered using the `register` function and If your token design allows for implicit registration, the `transfer` function could potentially register the recipient if they're not already registered. However, this might have security implications and should be carefully considered.

You can check that the `coin module` is accessible to the `transfer` function. Ensure there are no module access restrictions that might prevent the `is_account_registered` and `transfer` functions from being called.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Calling Supra Coin
From Supraframework do we have supra coins?

## ➥ SOLUTION SUMMARY:

Yes,
Use below call for calling Supra coin for the framework
```Move
use supra_framework::supra_coin;
```
