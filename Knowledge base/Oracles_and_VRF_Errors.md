## Table of Contents
1. [> If a request fails on callback, is there a time-delay before it is retried?](#issue-summary-if-a-request-fails-on-callback-is-there-a-time-delay-before-it-is-retried)
2. [> Time period For Request to be permanently dropped](#issue-summary-time-period-for-request-tobe-permanently-dropped)
3. [> Callback due to Insufficient Funds](#issue-summary--callback-due-to-insufficient-fund)
4. [> Pull Oracles are returning 404](#issue-summary-pull-oracles-are-returning-404)
5. [> Whitelisting Note for dVRF.](#issue-summary-whitelisting-note-for-dvrf)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

NOTE: Kindly follow the below format to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: If a request fails on callback, is there a time-delay before it is retried?

### ➥ SOLUTION SUMMARY:
Depends on the type of failure. There can be a lot of reasons for the failure.
If the client has insufficient balance then its going to get failed after 3 retries from each primary and backup. So we send multiple emails alerts before the client balance reaches minBalance.
If the delay is because of txn not getting confirmed yet and still in the mempool (because of the gas price) then we do retry twice with a span of 3 mins  from each primary and backup.
If the failure is because of some revert actions present in the client contract then we provide proper events to tranck them.

NOTE:: Backup node works 15 blocks behind primary

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Time period For Request to be permanently dropped
Is there a time window where, after a certain amount of retries or some time period, the request is permanently dropped?

### ➥ SOLUTION SUMMARY:
No specific time window in this version that will be available in the next version.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY:  Callback due to Insufficient Funds
If Chainlink ever fails its callback (due to insufficient funds or whatever the case may be), we're able to rescue funds from the game contract itself, knowing that after 24 hours, that specific request will never try to callback again. I was wondering if we had any guarantees in this way for Supra, so we know how to handle an event of something going wrong.

### ➥ SOLUTION SUMMARY:
For Chainlink if it fails only due to insufficient balance then it keeps the request for 24 hrs. They do that because they dont have restrictions while accepting the request but we dont accept any request if the client balance reaches the min balance but we serve the responses of accumulated requests that were in queue.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## ISSUE SUMMARY: Pull Oracles are returning 404.

### ➥ SOLUTION SUMMARY:
Using the code present in the master branch, Please ask follow this one: https://github.com/Entropy-Foundation/oracle-pull-example/tree/feat/DoraV2/rest/javascript

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Whitelisting Note for dVRF.

Getting below Message:

`
Supra dVRF V2 requires subscription to the service with a customer controlled wallet address to act as the main reference.
Therefore you must register your wallet with the Supra team if you plan to consume Supra dVRF V2 within your smart contracts.
Please refer to the Supra documentation for the latest steps on how to register your wallet for their service.
`

### ➥ SOLUTION SUMMARY:
It means you have to whitelist your address with Supra Team and then use dVRF.
