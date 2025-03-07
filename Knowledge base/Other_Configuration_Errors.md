## Table of Contents
[Extract an address using a private key](#issue-summary-extract-an-address-using-a-private-key)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

NOTE: Kindly follow the below format to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Extract an address using a private key

For some reason, when I try to extract an address using a private key, for example, with Ethereum, everything works correctly in testnet, but when I take the supra private key, I get a different address, not the one indicated in the application, they even differ in length, it is still very problematic with it, maybe the library is not suitable and maybe they have some kind of library for working with supra.

### ➥ SOLUTION SUMMARY:
The discrepancy you're encountering between Ethereum and Supra address derivation likely from fundamental differences in both chain's underlying cryptographic algorithms and address formats, you wanna look at the library again, eth addresses are 160bit, Supra has 256.
