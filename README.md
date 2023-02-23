# Build and Deploy a DAO on Moonbeam with Hardhat

This repository contains instructions for deploying a test DAO project to a local Moonbeam node
using Hardhat.

This is inspired by [Drnutsu's tutorial](https://medium.com/mates-moonbeam/build-your-first-dao-and-deploy-it-to-moonbeam-network-part-3-deploy-to-moonbeam-4bfa38fe1c44)
on a similar topic.

## Prerequisites

It is assumed that you have the following prerequisites installed:

* git
* yarn or npm
* [docker](https://docs.docker.com/get-docker/)

## Tutorial

### ROUGH OUTLINE

* Clone scaffold-eth
	* git clone https://github.com/scaffold-eth -b simple-DAO-proposals
	* cd
	* yarn install
	* export NODE_OPTIONS=--openssl-legacy-provider # may need...
	* yarn chain # runs an Ethereum node
* Prepare PowDAO.sol
	* review packages/hardhat/deploy/00_deploy_your_contract.js

* Change network params
	* edit packages/hardhat/hardhat.config.js
		* Add localMoonbeam: {/*...*/}
		* set `const defaultNetwork = "localMoonbeam";`
		* set DEBUG = true (we want it to print out the `deployer` address)
* Generate privkey
	* run `yarn generate`
	* copy `privateKey` and `Account Generated as` address
* Run moonbeam
	* TODO: show docker commands
* Send yourself some money
	* TODO: show how to run UI (and what about https vs ws?)
* Compile with hardhat
	* (remove? handled by deploy?)
* Deploy
	* run `yarn deploy`
	* chain needs to be running, so either:
		* moonbeam node running
		* run `yarn chain` in another terminal
	* (in another terminal): yarn deploy
* Interact
	* run `yarn start`
	* replace 8545 with 9933 in packages/react-app/src/constants.js
	* TODO

## References

[Moonbeam](https://moonbeam.network)
[Polkadot](https://polkadot.network/)
[Hardhat](https://hardhat.org/)
