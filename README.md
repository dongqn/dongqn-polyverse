# PIT Phase 1 repo project: Cross-chain Proof of NFT

This repository is created to enter the PIT phase 1 challenge # 22.

## Team Members

- @tmsdkeys - Lead Developer
- @dshiell - Developer

## Description

This application enables a user to submit a vote a ballot on a contract on OP Sepolia. The information about this vote subsequently gets sent to an ERC721 NFT contract on Base Sepolia to mint an NFT related to the vote.

Features:

- Uses Polymer x IBC as the cross-chain format
- Commits to the spirit of application specific chains/rollups where voting functionality could be specialized on one chain, NFT marketplace on another and both can form composable applications through interoperability.

## Resources used

The repo uses the [ibc-app-solidity-template](https://github.com/open-ibc/ibc-app-solidity-template) as starting point and adds custom contracts XBallot and XProofOfVoteNFT that implement the custom logic.

It changes the send-packet.js script slightly to adjust to the custom logic.

The expected behaviour from the template should still work but nevertheless we quickly review the steps for the user to test the application...
Run `just --list` for a full overview of the just commands.

Additional resources used:
- Hardhat
- Blockscout
- Tenderly

## Steps to reproduce

After cloning the repo, install dependencies:

```sh
just install
```

And add your private key to the .env file (rename it from .env.example).

Then make sure that the config has the right contracts:
```sh
just set-contracts optimism XBallot && \
just set-contracts base XProofOfVoteNFT
```

> Note: The order matters here! Make sure to have the exact configuration

Check if the contracts compile:
```sh
just compile
```
### Deployment and creating channels (optional)

Then you can deploy the contract if you want to have a custom version, but you can use the provided contract addresses that are prefilled in the config. If using the default, you can skip to the step to send packets.

If you want to deploy your own, run:
```sh
just deploy optimism base false
```
and create a channel:
```sh
just create-channel
```

### Sending a packet

Now with an existing channel in the config (your own or the default), run:

```sh
just send-packet optimism false
```
You'll see an active waiting poll in the terminal and will be informed if the packet was sent successfully.

## Proof of testnet interaction

After following the steps above you should have interacted with the testnet. You can check this at the [IBC Explorer](https://explorer.ethdenver.testnet.polymer.zone/).

Here's the data of our application:

- XBallot (OP Sepolia) : 0xB604C9F99Dc3Ebff9E12b71690141c7939fA0266
- XProofOfVoteNFT (Base Sepolia): 0x33e23218a21bF730CFb07822CDCDfb2B11e962A5
- Channel (OP Sepolia): channel-20
- Channel (Base Sepolia): channel-21

- Proof of Testnet interaction:
    - [SendTx](https://optimism-sepolia.blockscout.com/tx/0x11431bcd6e7799ba4db96a6da4a61e5a0c98cd41c41e4fb6749c3b6191c21f10)
    - [RecvTx](https://base-sepolia.blockscout.com/tx/0x73346e31af72d40a93076a7082d5dd099e2242b68e194912e227e269639067d2)
    - [Ack](https://optimism-sepolia.blockscout.com/tx/0x53c021bbd48958ce84f5185c0287228e573f0fcfd755ee9e2c2d605b534ca834)

## Challenges Faced

- Debugging used to be tricky when the sendPacket on the contract was successfully submitted but there was an error further down the packet lifecycle.
What helped was to verify the contracts and use Tenderly for step-by-step debugging to see what the relayers submitted to the dispatcher etc.

## What we learned

How to make the first dApp using Polymer.

## Future improvements

Basic functionality was implemented, but the following things can be improved:

- More tests
- More input validation
- Add event listeners related to important IBC lifecycle steps

## Licence

[Apache 2.0](LICENSE)
