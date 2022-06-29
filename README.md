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
