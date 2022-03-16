## ZSC Smart Contract NetWork

> A friendly blockchain network for smart contracts

**Version 1.0**

- [Background](#Background)
- [Design concept](#Design-concept)
- [Number of consensus and validators](#Number-of-consensus-and-validators)
  - [Proof Of Staked Authority](#Proof-Of-Staked-Authority)
  - [Quorum of validator nodes](#Quorum-of-validator-nodes)
  - [Safety and finality](#Safety-and-finality)
  - [Income](#Income)
- [Token Economics](#Token-economics)
  - [Native token](#Native-token)
  - [Other tokens](#Other-tokens)
- [Stakes and On-Chain Governance](#Staking-and-On-Chain-Governance)
  - [Equity-pledge](#Equity-pledge)
- [Outlook](#Outlook)

## Background

After the ZSC Smart Network launched its mainnet in April 2020, it demonstrated its high-speed, high-throughput design. Its main focus - a native decentralized application ("dApp") ZSC smart network DEX (Decentralized Exchange), has withstood the test of handling millions of transactions in a short period of time, demonstrating its trading engine low-latency matchmaking capability.

Flexibility and usability often go hand-in-hand with performance. When the focus is on how to provide a convenient place to issue and trade digital assets, this design also brings limitations to some extent. The community's highest voice has always been to see the ZSC smart network adding programmable extensions, or to put it bluntly, [smart contracts](https://en.wikipedia.org/wiki/Smart_contract) and virtual machine functions. Due to the limited functionality of the current ZSC smart network, digital asset issuers and owners have a headache when they want to add new decentralized features to their assets or introduce any form of community management and community activities.

Despite high expectations for adding smart contract functionality to the ZSC smart network, it was a "difficult decision". The execution of smart contracts can slow down DEX operations and add uncertainty to asset transactions. Even if this compromise can be tolerated, the easiest solution to think of may be to implement a virtual machine based on the current underlying Tendermint consensus protocol and the main RPC interface of the ZSC smart network, but this solution will increase the learning cost of the existing dApp community and will not is a welcome solution.

Here we propose the concept of a parallel blockchain of the ZSC smart network to maintain the high performance of the native DEX while friendly support for smart contract functions.

## Design-concept

The design concept of ZSC intelligent network follows the following principles:

1. **independent-blockchain**: Technically, the ZSC Smart Network is an independent blockchain, not a Layer 2 solution. Most of ZSC's underlying technology and business functions should operate independently and not be affected by any other blockchain network.

2. **Ethereum-Compatible**: The first practical and widely used smart contract platform was Ethereum. In order to connect with relatively mature applications and communities, ZSC Smart Network has chosen to be compatible with the existing Ethereum mainnet. This means that most **dApps**, ecosystem components and tools will be compatible with ZSC with no or only minor changes; ZSC nodes will only require similar, or slightly higher hardware specifications and operational skills to function . This implementation should provide room for continued compatibility between ZSC and future versions of Ethereum.

3. **Stakes-and-On-Chain-Governance**: Consensus based on pledge of equity (PoS) is more environmentally friendly and provides more flexible options for community governance. It can be expected that this consensus will have better performance than PoW consensus, i.e. shorter block time and higher transaction processing capacity.

## Number-of-consensus-and-validators

Based on the above design principles, the consensus protocol of ZSC intelligent network is to achieve the following goals:

1. The block time should be shorter than the Ethereum time, only 3 seconds or less.
2. Only need to wait a limited time for the transaction to be finalized, such as about 1 minute or less.
3. There is no inflation, the revenue of the blockchain comes from the handling fee, and the handling fee is paid in the form of ZTB.
4. Be as compatible with Ethereum as possible.
5. Equipped with an on-chain governance mechanism based on equity pledge.

### Proof-Of-Staked-Authority

Although Proof of Work (PoW) has proven to be a practical solution for implementing a decentralized network, it is not environmentally friendly and requires a large number of participants to maintain network security.

Ethereum and some other networks like MATIC Bor, TOMOChain, GoChain, xDAI use Proof of Authority (PoA) or its variants in different scenarios, including testnet and mainnet. PoA provides defense against 51% attacks and more effectively prevents some Byzantine nodes from doing evil. Choosing PoA as the underlying consensus is one of the ideal options.

At the same time, PoA protocols have been criticized for being less decentralized than PoW, as validators, the nodes who take turns producing blocks, have enormous power and are prone to corruption and security attacks. Other blockchains, such as EOS, have introduced a different type of Delegated Proof of Stake (DPoS) that allows token holders to vote for validator nodes. It makes the network more decentralized and facilitates community management.

Inspired by the above, ZSC adopts the DPoS consensus, and the adopted scheme is:

1. Blocks are generated by a limited number of validators
2. Validators take turns generating blocks in a POS manner, similar to Ethereum's Clique consensus engine
3. The validator set is selected and eliminated based on the on-chain governance of equity pledge

### Validator Node Quorum

During the genesis block phase of network launch, some trusted nodes will operate as the initial set of validators. After the block starts, anyone can participate as a candidate to run as a validator. The staking status determines that the top 4 nodes with the most stakes become the next validator set.

**ZTB is the token of ZSC chain equity pledge**

To maintain compatibility with the Ethereum consensus protocol (including upcoming upgrades), there is a module dedicated to ZSC staking on the ZSC Smart Network. It will accept the pledge of ZSC rights and interests of ZTB holders, and enjoy the pledged mining to obtain ZTB rewards.

### Security and finality

Considering that more than half of the ½\*N+1 validators are honest and trustworthy, PoA-based networks generally work safely and normally. In some cases, however, Byzantine validators may still manage to attack the network, such as through a "clone attack". To ensure that ZSC has the same high level of security as BC, we encourage ZSC users to wait until the received block is confirmed by more than ⅔\*N+1 different validators. In this way, ZSC can achieve a similar level of security as BC and can tolerate less than 1/3 \*N Byzantine validators.

For 21 validators, if the block time is 5 seconds, then ⅔\* N + 1 different validator confirmations will take (2/3\*21 + 1)\*5 = 75 seconds of time. Any significant application of ZSC may have to wait ⅔*N + 1 to ensure a relatively safe final result. However, in addition to this arrangement, ZSC also introduces a penalty mechanism to penalize Byzantine validators for double-signature or instability, which will be covered later in the "Staking and Governance" section. This penalty mechanism will expose malicious validators for a short period of time and make the "clone attack" very difficult or uneconomical to execute. With this penalty mechanism, ½\*N + 1 or less blocks are sufficient for most transaction finality.

### Income

All ZSC validators in the current validator set will receive revenue from fees paid in ZTB. Since ZTB is not an inflationary token, it will not generate mining income like Bitcoin and Ethereum networks, and the fee is the main income of the validator. Since ZTB is also a utility token for other applications, delegators and validators will still receive the other benefits of holding ZTB.

The validator's revenue is obtained from the fees collected from each block's transactions. Validators can decide how much to share with delegators who have staked ZTB to attract more staked investments. Each validator will take turns producing blocks with the same probability (if they stay 100% online), so all stable validators are likely to gain a similar scale in the long run. At the same time, the staked asset size may be different for each validator, so this brings about a counter-intuitive situation where the more users trust and delegate staking to the same validator, they may earn less . Therefore, as long as the validator is still trusted (untrusted validators can be extremely risky), rational delegators will tend to delegate validators with lower stakes. Ultimately, all validators will be less differentiated. This will actually prevent the stake concentration and "winner always wins" issues that have arisen on other networks.

A portion of the fee will also be rewarded to relayers communicating across chains. Please refer to the "Repeaters" section below.

## Token-Economics

ZTB and ERC20 tokens can be circulated on ZSC at the same time. This defines:

1. Tokens can be created by deploying smart contracts directly on ZSC, in a format similar to ERC20 .
2. ZTB ​​is the native token of the ZSC intelligent network and the underlying basic token of the ZSC intelligent network.

### Native-token

ZTB works on ZSC in the same way that ETH works on Ethereum, so it is the "native token" of ZSC. This means that in addition to ZTB being used to pay most of the fees on ZSC Smart Network and ZSC Smart Network DEX, ZTB will also be used to:

1. Pay fees for deploying smart contracts on ZSC
2. Stake the selected ZSC validator and get the corresponding rewards
3. Perform cross-chain operations, such as transferring token assets across ETH (or BSC) and ZSC

**Start-up fund**

During the startup phase, a certain amount of ZTB will be destroyed on BC and then transferred to ZSC. This amount is called the "Startup Fund", it will be recorded in the first block of ZSC, it will be transferred to the initial BC-to-ZSC relay account (described in a later section) and initial verification people on the collection account. These ZTBs are used to pay transaction fees at an early stage to transfer more ZTBs from BC to ZSC through a cross-chain mechanism.

ZTB cross-chain transfers will be discussed in the following subsections, but for BC to ZSC transfers, it is common to lock the ZTB on the BC from the source address of the transfer to a system-controlled address, and unlock the corresponding amount of ZTB from the special contract to The destination address for transfers on ZSC, or conversely, when switching from ZSC to BC, is to lock ZTB from the source address on ZSC into a special contract and release the locked ZTB on BC from the system address to the destination address. This logic is implemented by code on BC and a series of smart contracts on ZSC.

### Other-tokens

Since ZSC is Ethereum compatible, supporting ERC20 contracts on ZSC is called "ZRC20". ZRC20 "enhances" existing protocols by adding more methods that expose more information, such as token units, precision. Token owners are able to decide the cross-chain token binding address.

**Token-binding**


## Cross-chain-transfer-and-communication

Cross-chain communication is the key infrastructure that allows the community to take advantage of the dual-chain structure:

- Users can freely create any tokenized financial products and digital assets on ZSC or BC;
- Tokens on ZSC can complete programmed transactions and asset circulation in a stable, high-throughput, extremely fast and friendly environment on BC;
- Users can do this in a unified tool ecosystem and user interface.

### Cross-chain-transfer

Cross-chain transfers are the key to communication between two blockchains. Its logic is:

1. The transferred blockchain will lock the amount held from the source address to the address/contract controlled by the system;
2. Transferring to the blockchain will unlock the amount in the address/contract controlled by the system and transfer it to the destination address.

The cross-chain transfer packet message should allow the ZSC relay and the BC Oracle relay to verify the following information:

1. A sufficient amount of token assets are locked from the source address to the system control address/contract on the source blockchain. This can be confirmed on the target blockchain.
2. The appropriate amount of token assets is released from the address/contract controlled by the system and allocated to the target address on the target blockchain. If it fails, it can be confirmed on the source blockchain so that the locked tokens can be released back (possibly with a fee deducted).
3. No matter whether the transfer is successful or not, after the transfer operation is completed, the total amount of token assets in circulation on the two blockchains will not change.

The architecture of cross-chain communication is shown in the figure above. To accommodate two heterogeneous systems, the communication processing is different in each direction.


### Cross-chain-user-experience

Ideally, users want to use two parachains the same way they would use a single chain. It would require adding more composite transaction types to cross-chain communication to support this, which would greatly increase complexity, tight coupling, and maintenance burden. Here, BC and ZSC implement only basic operations to facilitate the flow of value at initial launch, and implement most of the user experience work through the client, such as wallets. For example, a wallet could allow users to send tokens directly from ZSC to BC's DEX to sell tokens in a safe and secure way.

### Cross-chain-contract-events

Cross-chain contract events (CCCE) are designed to allow smart contracts to trigger cross-chain events directly through contract code. This is possible on the basis of:
1. Standard system contracts can be provided to serve operations that can be invoked by general smart contracts;
2. Standard events can be issued by standard contracts;
3. Oracle repeaters can capture standard events and trigger corresponding cross-chain operations;
4. A dedicated, code-managed address can be created on the BC and accessed by the ZSC contract, here it is named "BC's Contract Address" (CAoB).

The following typical standard functions will be implemented:

Implement the following standard operations:

1. ZSC to ETH (or BSC) transfer: This is triggered by a standard contract just like a normal ZSC to ETH (or BSC) transfer. Funds can be transferred to any address on ETH (or BSC), including transfers to the corresponding CAoB in the original contract.
2. Intra-ETH (BSC) transfer: The completion of the transfer is considered as a special cross-chain transfer, while the real transfer is from CAoB to any other address (even another CAoB).
3. ETH (BSC) to ZSC conversion: This is achieved by means of dual-pass cross-chain communication. The first channel is triggered by the ZSC contract and transferred to ETH(BSC), then in the second channel, ETH(BSC) will initiate a normal cross-chain transfer from ETH (or BSC) to ZSC, transfer address It is the contract address from CAoB to ZSC. It is important to note that the ZSC contract only increases the balance when the second channel transfers, and the error handling in the second channel transfer is the same as the normal ETH(BSC) to ZSC transfer.
4. IOC (Immediately Execute or Cancel) Transactions: The main purpose of transferring assets to ETH(BSC) is trading. This event will instruct to trade one asset in the CAoB for another, transfer the remaining target tokens of the transaction, and transfer the full result to ZSC. ETH(BSC) will handle such relay events by sending "IOC" orders, such as IOC orders are sent to the trading pair, once the next match is completed, the execution result will be sent back to ZSC, which can be an asset, or Can be two assets.

Here are some details of the deal:

a. Both parties may have a price limit (absolute or relative) for the transaction;
b. The final result will be written to ZSC in the form of a cross-chain transaction package;
c. Cross-chain communication fees can be charged from the assets transferred back to ZSC;
d. The ZSC contract reflects the CAoB's balance and outstanding orders. Whatever errors occur during the transaction, the final state will be transferred back to the original contract and its internal state will be liquidated.

On the basis of the above characteristics, it is very convenient to add cross-chain transfer and transaction functions with high liquidity to all smart contracts of ZSC. It will greatly increase the application scenarios on smart contracts and dApps, enabling Binance Dual Chain to produce a 1+1>2 aggregation effect.

## Staking-and-On-Chain-Governance

PoSA realizes decentralized community governance. You can see similar ideas from other networks, notably Cosmos and EOS. Its core logic can be summarized as follows.

1. Token holders, including validators, can “lock” their tokens into staking. Token holders can **delegate** their tokens to any validator or a validator candidate. They can also later re-select a different validator or candidate to delegate their tokens.
2. All validator candidates will be sorted by the number of delegated tokens they have obtained, and the top ones will become the real validators.
3. Validators can share block rewards with their delegators.
4. Validators may suffer **“penalty interest”**, i.e. penalties for their bad behavior such as double signing and/or instability. Such losses are also shared with their principals.
5. Validators and delegators have an "unbinding period". When malicious Byzantine behavior is discovered, the tokens will remain locked for a certain period of time, and the perpetrator will be punished in time.


### Equity-pledge

Ideally, such staking and reward distribution logic should be included in the blockchain and executed automatically when new blocks are produced. This is how the Cosmos Hub, which uses the Tendermint consensus library as the ZSC smart network, works.

ZSC has been preparing to enable PoS since the day it was designed. On the other hand, ZSC wants to be as compatible with Ethereum as possible, and it is a huge challenge and pressure to directly implement PoSA on it. This is especially true considering that Ethereum itself may migrate to a PoS consensus protocol within a short time (or longer). In order to maintain the compatibility of Ethereum and reuse the infrastructure of BC, we have completed the staking logic of ZSC on BC:

1. The pledged token is ZTB because it is the native token on both blockchains.
2. Record ZSC's equity pledge and delegation behavior on BC.
3. The ZSC validator set is determined by its equity pledge and delegation logic. A ZSC equity pledge module is constructed on BC, and is transmitted from BC to ZSC at UTC 00:00:00 every day through cross-chain communication.
4. Reward distribution on BC occurs at UTC 00:00 every day.

## Outlook

For the future of the ZSC intelligent network, it is difficult to make a conclusion, because it has never stopped evolving. At present, it is only a stage of the development of ZSC intelligent network. Here are some topics worth paying attention to to promote better usability and scalability for the community:

1. Add different digital asset models for different business use cases, richer financial models on the chain.

2. Realize more data sources, especially DEX market data, to capture more ZSC chain data information.

3. Provide interfaces compatible with Ethereum and its future upgrades, as well as other blockchains. Compatible with more blockchain underlying networks.

4. Improve the usability of wallets and blockchain clients, and optimize usability.




