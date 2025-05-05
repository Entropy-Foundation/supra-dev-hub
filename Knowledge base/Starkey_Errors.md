## Table of Contents
1. [> Starkey Wallet Connection in Next.js](#ISSUE-SUMMARY-starkey-wallet-connection-in-nextjs)
2. [> Sign function code snippet for Wallet](#ISSUE-SUMMARY-sign-function-code-snippet-for-wallet)
3. [> Metadata retrieval for their custom token](#ISSUE-SUMMARY-starkey-wallet-or-explorer-metadata-retrieval-for-their-custom-token)
4. [> Difference in Balance on RPC and Explorer](#ISSUE-SUMMARY-difference-in-balance-on-rpc-and-explorer)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

NOTE: Kindly follow the below format to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: StarKey Wallet Connection in Next.js
Share a sample code for StarKey Wallet connection in next.js

### ➥ SOLUTION SUMMARY:

**Demo Here:** https://frontend-web-wallet-connect-demo.vercel.app/supra-dapp

**Repo:** https://github.com/nolan-supra/starkey-demo

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Sign function code snippet for Wallet

### ➥ SOLUTION SUMMARY:
signMessage accepts a hex string as message and returns object containing:
address of signer, public key, and signature

```TypeScript
import nacl from "tweetnacl";

const remove0xPrefix = (hexString: string) => {
  return hexString.startsWith("0x") ? hexString.slice(2) : hexString;
}

const hexString = '0x' + Buffer.from(signMessage, 'utf8').toString('hex');
const response = await supraProvider.signMessage({ message: hexString });
console.log('signMessage response :: ', response);

if (response) {
  const { publicKey, signature, address } = response;
  const sign = remove0xPrefix(signature);
  const key = remove0xPrefix(publicKey);
  const verified = nacl.sign.detached.verify(
    new TextEncoder().encode(signMessage),
    Uint8Array.from(Buffer.from(sign, 'hex')),
    Uint8Array.from(Buffer.from(key, 'hex')),
  );
  console.log('signature :: ', signature);
  console.log('verified :: ', verified);
}
```

You can test in your browser dev console with the following:

```TypeScript
const response = await window.starkey.supra.signMessage({
  message: "0x74657374",
});
```
![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: StarKey Wallet or explorer Metadata retrieval for their custom token

### ➥ SOLUTION SUMMARY:

```
https://rpc-testnet.supra.com/rpc/v1/accounts/0xa9aa1587371e78286391d86d7e951cf2c177e5d48eb494387e74eefcc53d6217/resources/0x1::coin::CoinInfo%3C0xa9aa1587371e78286391d86d7e951cf2c177e5d48eb494387e74eefcc53d6217::moon_coin::MoonCoin%3E
```

- Coins have a CoinInfo resource.
- You query for the CoinInfo to retrieve the metadata.

When you add it to StarKey, you add it as: `a9aa1587371e78286391d86d7e951cf2c177e5d48eb494387e74eefcc53d6217::moon_coin::MoonCoin`

and the wallet will query the CoinInfo resource to grab the metadata for the token and display it in walletCoinInfo in coin.move:

- https://github.com/Entropy-Foundation/aptos-core/blob/f9652d1f0472fc60f887605ff13f[…]01b039e4/aptos-move/framework/supra-framework/sources/coin.move

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Difference in Balance on RPC and Explorer
When I try to get the balance using sdk it shows 200000000, but in the explorer it shows 100000000 supra coin. Why?

### ➥ SOLUTION SUMMARY:
This will be displayed on StarKey but not on Suprascan. It's shown to you as output, but it will not be in the RPC or Exploror. Once this is unstaked or the time of the staking is over, then when that amount is again credited to their StarKey Wallet, that activity will be tracked, and then this balance will be shown on a Suprascan. But not now until you have stacked your thing.
