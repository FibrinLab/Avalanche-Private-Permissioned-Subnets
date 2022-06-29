# Creating a Private, Permissioned Avalanche Subnet (for Health Data Management)

## Introduction

Avalanche is an open-source platform for launching decentralized applications and enterprise blockchain deployments in one interoperable, highly scalable ecosystem. Avalanche is the first decentralized smart contracts platform built for the scale of global finance, with near-instant transaction finality. 

Blockchains have traditionally been referred to as being slow and unscalable. Avalanche embraces an innovative approach to concensus that solve these problems without compromising on security.

Avalanche is a high-performance, scalable, customizable, and secure blockchain platform. It targets three
15 broad use cases:

* Building application-specific blockchains, spanning permissioned (private) and permissionless (public)
deployments.
* Building and launching highly scalable and decentralized applications (Dapps).
* Building arbitrarily complex digital assets with custom rules, covenants, and riders (smart assets).

Avalanche features 3 built-in blockchains: 
* Exchange Chain (X-Chain)
* Platform Chain (P-Chain)
* Contract Chain (C-Chain)

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



## Private Subnets for Managing Healthcare Data
The challenges and risks related to centralized health records can be basically summarised as vulnerablity and access management difficulties. Various bodies has studied and fully comprehened the risks involved in data loss in [centralised systems](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3068075/). 
Among other things, this conclusion led the US Department of Defense to create in 1969 the ARPANET, a network system that would be able to remain functional in case of a catastrophe. Regarding health data, the same approach can be applied and it would be obviously much safer to store such data in a decentralised permissioned fashion to ensure the protection of information, as it could be, for example, a target for terrorists who wish to destabilize a country by destroying or by pirating its health system and by divulging health information on citizens. Furthermore, hackers may see a centralized system as a challenge and try to gain access to a centralized patient EMR system and modify patients' medical information.



## Prerequisites

### NodeJS and Yarn

First, install the LTS (long-term support) version of [nodejs](https://nodejs.org/en). This is `16.2.0` at the time of writing. NodeJS bundles `npm`.

Next, install the [yarn](https://yarnpkg.com) package manager:

```zsh
npm install -g yarn
```


# 1. Creating an EVM Subnet on the Local Network using the default configuration.

The first step would be to download and set up the the [Avalanche-CLI](https://github.com/ava-labs/avalanche-cli).

> Please note that the Avalanche-CLI currently in active development. Commands may be renamed or removed in future versions. Examples and their output may become outdated. Please check this page and the github project often for more updated instructions.

## Step 1: Installation

Create an empty folder in the main directory and `change directory (cd)` into it. Then proceed to running the following command.

```zsh
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
```
The binary will be installed relative to where the command was run.

Next add the binary to the path by running the following command

```zsh
cd bin
export PATH=$PWD:$PATH
```

## Set 2: Initilizing a default subnet

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