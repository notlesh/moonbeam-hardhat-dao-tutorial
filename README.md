# Build and Deploy a DAO on Moonbeam with Hardhat

This repository contains instructions for deploying a test DAO project to a local Moonbeam node
using Hardhat.

This is inspired by [Drnutsu's tutorial](https://medium.com/mates-moonbeam/build-your-first-dao-and-deploy-it-to-moonbeam-network-part-3-deploy-to-moonbeam-4bfa38fe1c44)
on a similar topic.

## Prerequisites

It is assumed that you have the following prerequisites installed:

* git
* node
* yarn or npm
* [docker](https://docs.docker.com/get-docker/)

## Tutorial

* Clone scaffold-eth
	```bash
	# this repo uses different branches for different projects
	# so if you want to explore it, look at the different branches
	# we use '--single-branch' to reduce the download/disk usage impact
	git clone https://github.com/scaffold-eth/scaffold-eth -b simple-DAO-proposals --single-branch

	cd scaffold-eth
	yarn install
	```

* Prepare Moonbeam
	This can take some time to download, so we'll kick that off now
	```bash
	docker pull purestake/moonbeam:v0.29.0
	```

* Prepare PowDAO.sol
	* (TODO: explain the interesting bits here)
	* review packages/hardhat/deploy/00_deploy_your_contract.js
	* (TODO: replace keys)

* Add local moonbeam node as network in hardhat config
	* edit packages/hardhat/hardhat.config.js
		* Add localMoonbeam: {/*...*/} (TODO)
		* set `const defaultNetwork = "localMoonbeam";`
		* set DEBUG = true (we want it to print out the `deployer` address)
* Generate privkey
	```bash
	yarn generate
	# copy 'privateKey' and 'Account Generated as' address
	```
* Send yourself some money
	* TODO: show how to run UI (and what about https vs ws?)
* Compile with hardhat
	* (remove? handled by deploy?)

* Run Moonbeam
	Moonbeam can easily be run on Linux, but we use Docker to make it painless to run cross-platform.

	Full documentation: https://docs.moonbeam.network/builders/get-started/networks/moonbeam-dev/

	```bash
	# Linux via Docker:
	docker run --rm --name moonbeam_development --network host \
	purestake/moonbeam:v0.29.0 \
	--dev

	# Linux via raw binary:
	./moonbeam --dev

	# MacOS:
	docker run --rm --name moonbeam_development -p 9944:9944 -p 9933:9933 \
	purestake/moonbeam:v0.29.0 \
	--dev --ws-external --rpc-external

	# Windows:
	docker run --rm --name moonbeam_development -p 9944:9944 -p 9933:9933 ^
	purestake/moonbeam:v0.29.0 ^
	--dev --ws-external --rpc-external
	```
* Deploy
	```bash
	# chain needs to be running, so either:
	# have moonbeam node running (see above)
	# or we can run `yarn chain` in another terminal to run Hardhat's built in Ethereum node

	export NODE_OPTIONS=--openssl-legacy-provider # may need
	yarn deploy
	```

* Modify MetaMask's RPC 
	* TODO:

* Interact

	Change port used for RPC:

	```bash
	# replace 8545 with 9933 in packages/react-app/src/constants.js

	# kicks off the web UI
	yarn start
	```
	* TODO: 

## References

* [Moonbeam](https://moonbeam.network)
* [Polkadot](https://polkadot.network/)
* [Hardhat](https://hardhat.org/)
