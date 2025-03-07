## Table of Contents
1. [> Client-side integration using supra sdk](#issue-summary-client-side-integration-using-supra-sdk)
2. [> Check getAccountModule](#issue-summary-check-getaccountmodule)
3. [> Verify Signature](#issue-summary-verify-signature)
4. [> Type error: Cannot find module 'supra-l1-sdk'](#issue-summary-type-error-cannot-find-module-supra-l1-sdk)
5. [> TYPE_RESOLUTION_FAILURE](#issue-summary-type_resolution_failure)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

NOTE: Kindly follow the below format to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Client-side integration using supra sdk
Integration for Module deployed on supra and integrated with client-side using supra sdk

### ➥ SOLUTION SUMMARY:
Relevant Links:
- https://sdk-docs.supra.com/classes/SupraClient.html
- https://github.com/Entropy-Foundation/supra-l1-sdk/blob/master/src/example.ts

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Check `getAccountModule`

In aptos sdk we have aptos.getAccountModule to check the module name do we have in supra sdk to check getAccountModule?

### ➥ SOLUTION SUMMARY:
The get account info function here returns the accounts auth key and sequence number:
- https://sdk-docs.supra.com/classes/SupraClient.html#getAccountInfo

Also u can check deployed module via API:

```
https://rpc-testnet.supra.com/rpc/v1/accounts/{address}/modules/{module_name}
```
![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Verify Signature

How to verify signature using supra sdk?

### ➥ SOLUTION SUMMARY:
- https://sdk-docs.supra.com/classes/SupraAccount.html#verifySignature

Here is a link to the verifySignature function code from the `SupraAccount` class (imported from AptosAccount):

- https://github.com/Entropy-Foundation/aptos-core/blob/c5087137d9c9a85975c368894a24ae647fc49285/ecosystem/typescript/sdk/src/account/aptos_account.ts#L170C1-L179C4

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Type error: Cannot find module 'supra-l1-sdk'
In a NextJS application, Facing a Type error: Cannot find module 'supra-l1-sdk' or its corresponding type declarations.

### ➥ SOLUTION SUMMARY:
Update the `tsconfig` file, Change `moduleResolution` from `bundler` to `node`

```
"moduleResolution": "node",
```
It will still have a warning, but the build will be working.

Below is the warning you will get:
```
The generated code contains 'async/await' because this module is using "topLevelAwait".
However, your target environment does not appear to support 'async/await'.
As a result, the code may not run as expected or may cause runtime errors.

Import trace for requested module:
./src/libs/supra.ts
```
Supra have made changes in the support branch as well for this so devs don't have to do this:
- https://github.com/Entropy-Foundation/supra-l1-sdk/tree/support-ledger-wallet

Devs can check the commits of 9th Dec'24 to check the changes made in that branch to replicate the same in your side of the code.

## ISSUE SUMMARY: TYPE_RESOLUTION_FAILURE
The below does not allow me to pass a string for the type argument. How should I format it there?

```TypeScript
let rawBuyTransaction = await supraClient.createRawTxObject(
  senderAccount.address(),
  (await supraClient.getAccountInfo(senderAccount.address())).sequence_number,
  config.DEX_CONTRACT,
  "router",
  "swap_exact_input",
  [],
  []
);
```

### ➥ SOLUTION SUMMARY:
Try the below approach

```TypeScript
// To create a serialized raw transaction
let supraCoinTransferSerializedRawTransaction =
await supraClient.createSerializedRawTxObject(
  senderAccount.address(),
  (
    await supraClient.getAccountInfo(senderAccount.address())
  ).sequence_number,
  "0000000000000000000000000000000000000000000000000000000000000001",
  "supra_account",
  "transfer_coins",
  [new TxnBuilderTypes.TypeTagParser("0x1::supra_coin::SupraCoin").parseTypeTag()],
  [new HexString("0x4d58f6c00e4902d28c4dfada7bbe46daf19e73033b0b05f02b54179353fa737b").toUint8Array(), BCS.bcsSerializeUint64(1000)]
);
```


**NOW,**

`
If TxnBuilderTypes cannot be imported from the SDK, then try the import below:
`

```TypeScript
import {
  SupraAccount,
  HexString,
  SupraClient,
  BCS,
  TxnBuilderTypes,
} from "supra-l1-sdk";

async function main() {
  const senderAccount = new SupraAccount(Buffer.from("", "hex"));

  let supraClient = await SupraClient.init("https://rpc-testnet.supra.com/");

  // To create a serialized raw transaction
  let supraCoinTransferSerializedRawTransaction =
    await supraClient.createSerializedRawTxObject(
      senderAccount.address(),
      (await supraClient.getAccountInfo(senderAccount.address())).sequence_number,
      "0000000000000000000000000000000000000000000000000000000000000001",
      "supra_account",
      "transfer_coins",
      [
        new TxnBuilderTypes.TypeTagParser(
          "0x1::supra_coin::SupraCoin"
        ).parseTypeTag(),
      ],
      [new HexString("").toUint8Array(), BCS.bcsSerializeUint64(1000)]
    );

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
}

main();
```
