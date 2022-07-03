# Creating a Private, Permissioned Avalanche Subnet (for Health Data Management)

## Introduction

Avalanche is an open-source platform for launching decentralized applications and enterprise blockchain deployments in one interoperable, highly scalable ecosystem. Avalanche is the first decentralized smart contracts platform built for the scale of global finance, with near-instant transaction finality. Avalanche is a blockchain that promises to combine scaling capabilities and quick confirmation times through its Avalanche Consensus Protocol. It can process 4,500 TPS (transactions per second). For Ethereum, that number is 14 TPS.

Blockchains have traditionally been referred to as being slow and unscalable. Avalanche embraces an innovative approach to concensus that solve these problems without compromising on security.

Avalanche is a high-performance, scalable, customizable, and secure blockchain platform. It targets three 15 broad use cases:

* Building application-specific blockchains, spanning permissioned (private) and permissionless (public)
deployments.
* Building and launching highly scalable and decentralized applications (Dapps).
* Building arbitrarily complex digital assets with custom rules, covenants, and riders (smart assets).

# Avalanche features 3 built-in blockchains: 
* Exchange Chain (X-Chain)
* Platform Chain (P-Chain)
* Contract Chain (C-Chain)

The P-chain is for platform management. It handles requests related to the validator, the subnet, and the blockchain. 
The C-chain is for contract management. It is based on EVM; hence its API is almost identical to other EVM protocols. It has both RPC and WebSocket endpoints, and it handles requests for smart contracts. 
The X-chain is for asset exchange. It is Avalanche’s native platform; it is used for creating and trading assets like AVAX and NFTs. 

These 3 blockchains are secured by the Avalanche Primary Network with is a special kind of subnet.

The Avalanche Architecture is composed of:
* Subnetworks
* Virtual Machines


The goal of this guide is to lay out best practices regarding setting up Private and Permissioned Avalanche's Subnet with the following levels of privace:
* Control of validator requirements
* Control of contract deployment
* Private transaction posting
* Private transaction reading


## Subnetworks
A subnet is defined as a set of validators working together to achieve a certain concensus on the state of a blockchain. Each blockchain is validated by one subnet, and a subnet can validate arbitrarily many blockchains.
Subnets have various advantages which include:
* Subnets can specify rules for members joining them, this is achieved via creating private subnets. This enables the blockchain to be governed only be a group of trusted validators.
* With this model, one can create subnets with various properties. For example, only allowing members from a certain jurisdiction or goegraphical location.
* With this model, one can create subnets with various properties. For example:
    * Validators must be located in a given country
    * Validators must pass a KYC/AML checks
    * Validators must hold a certain license

![subnets](/images/avax.jpeg "subnets")



## Private Subnets for Managing Healthcare Data
The challenges and risks related to centralized health records can be basically summarised as vulnerablity and access management difficulties. Various bodies has studied and fully comprehened the risks involved in data loss in [centralised systems](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3068075/). 
Among other things, this conclusion led the US Department of Defense to create in 1969 the ARPANET, a network system that would be able to remain functional in case of a catastrophe. Regarding health data, the same approach can be applied and it would be obviously much safer to store such data in a decentralised permissioned fashion to ensure the protection of information, as it could be, for example, a target for terrorists who wish to destabilize a country by destroying or by pirating its health system and by divulging health information on citizens. Furthermore, hackers may see a centralized system as a challenge and try to gain access to a centralized patient EMR system and modify patients' medical information.

# Roadmap

This tutorial is created to serve as a guide to enable developers create fully customizable Avalanche subnets. For the purpose of this tutorial, all testing will be done on the Local and Fiji testnet. This guide is an extension of the [Official Avalanche Documentation]().

We will be creating a blockchain network specific for managing healthcare data and transactions. This network would have the ablity to regulate it validator requirements and ensure a certain degree of privacy as if necessary for managing sensitive data.

Please note that all command line inputs and sample codes are MacOs and Linux Based. Commands may vary for other operating systems.

In summary, we will be discussing the following:
1. Running an EVM Subnet on a Local Network
2. Restricting Smart Contract Deployment on the Local Network
3. Running an EVM Subnet on Fuji Testnet
4. Private Transaction Posting and Reading
5. Specifying Validator Requirement


## Prerequisites

### NodeJS and Yarn

First, install the LTS (long-term support) version of [nodejs](https://nodejs.org/en). This is `16.2.0` at the time of writing. NodeJS bundles `npm`.

Next, install the [yarn](https://yarnpkg.com) package manager:

```zsh
npm install -g yarn
```

### Git

To check the current Git version use:

```zsh
git --version
```

# 1. Running an EVM Subnet on the Local Network using the default configuration

We will be creating an EVM on our local machine to give us a basic feel on how a subnet functions. The [Avalanche-CLI](https://github.com/ava-labs/avalanche-cli) is a novel tool that allow up to have a local network up in minutes.


## Step 1: Installation

Open up you MacOs command line utility and run the following command

On your home directory, create a new and `cd <newdir>` into the directory. This is where we will be installing all our project dependencies.

```zsh
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
```

This command download the latest binary of the [Avalanche-cli] to the current directory where it was executed.

`cd` into the `bin` folder and export the path variable

```zsh
cd bin
export PATH=$PWD:PATH
```

This makes the `avalanche` command available globally. For more information about [environment-variables]() and [avalanche-cli-commands]() visit the respective links.

![variables](/images/cover.jpeg "variables")


## Set 2: Initilizing a default subnet

We will be using the `avalanche subnet create` command line wizard to get our network running. STAT.
In the same directory where the binary was installed, run the following command

```zsh
avalanche subnet create <subnetName>
```
Substitute `<subnetName>` with any perferred name of your choice but without spaces. For this tutorial we are going to call our subnet `<fibrinNet>`.

```zsh
avalanche subnet create fibrinNet
```

Since this command does not provide any arguements, you would neeed to walk through the configuration wizard to generate a genesis file for your network.

* Choose a VM: 
  ![choose a VM](/images/2.png "Choose VM")
  We are going to be making use of the `SubnetEVM`

* Pick a chain ID
  ![chain ID](/images/3.png "Chain ID")
  Every EVM based blockchain has a special parameter called a `chainID`. ChainIDs are expected to be unique values, you can check [chainlist.org](https://chainlist.org/) to see if your proposed chain ID is already in use. We will be making use of the chain ID `1970` (A pun on JavaScript dates...lol).

* Select a symbol for the native subnet token
  ![symbol](/images/4.png "token symbol")

* Set fees: Select the `low disk use / low throughput` option
  ![fees](/images/5.png "fees")

* Airdrop: default to airdrop 1 million tokens to provided address
  ![airdrop](/images/6.png "airdrop")

* Add a custom precompile to modify the EVM: For this section, we will not be using a pre-compile script
  ![precompile](/images/7.png "precompile")


The wizard won't customize every aspect of the Subnet-EVM genesis for you, we will be doing this in the subsequent sections.

![complete](/images/8.png "complete")

To view the list of all created subnets, just execute the following command

```zsh
avalanche subnet list
```
![list](/images/9.png "list")


## Step 3: Deploying the Subnet Locally.

To deploy the newly created subnet locally, run the following command

```zsh
avalanche subnet deploy <subnetName>
```

![deploy](/images/10.png "deploy")

When a subnet is run locally, it starts a multi-node Avalanche Network in the background.

![deploy_f](/images/11.png "deploy_f")

To test the functionality of the just created subnet, go ahead and add the configuration details to [Metamask](https://metamask.io/).
You can create a new metamask account by importing the private key `0x56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027` and start experiencing with this account.

Lastly don't forget to stop the running local network
```zsh
avalanche network stop <snapshotName>
```

![deploy_f](/images/12.png "deploy_f")


For detailed instructions on how to set-up the new RPC network to work with metamask, visit the Avalanche [Documentation](https://docs.avax.network/subnets/deploy-a-smart-contract-on-your-evm#step-1-setting-up-metamask)

![metamask](/images/metamask.png "metamask")


## Step 4: Deploying a Test Smart Contract on the Subnet.

From your preferred web browser, navigate to the [Remix](https://remix.ethereum.org/) Platform.
We will be importing a sample solidity code from a git