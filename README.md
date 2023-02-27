# Build and Deploy a DAO on Moonbeam with Hardhat

This repository contains instructions for deploying a test DAO project to a local [Moonbeam](https://moonbeam.network/) node
using [Hardhat](https://hardhat.org/).

We start with [scaffold-eth](https://github.com/scaffold-eth/scaffold-eth), a very complete Ethereum project template, and modify it to work with Moonbeam.
Then we deploy its sample "PowDAO" project and explore it.

This is inspired by [Drnutsu's tutorial](https://medium.com/mates-moonbeam/build-your-first-dao-and-deploy-it-to-moonbeam-network-part-3-deploy-to-moonbeam-4bfa38fe1c44)
on a similar topic.

## Prerequisites

It is assumed that you have the following prerequisites installed.
These are the same as found in `scaffold-eth` with the addition of Docker to help us run Moonbeam without worrying about our OS environment.

* git
* node
* yarn (`npm i --global yarn`)
* [docker](https://docs.docker.com/get-docker/)
* [MetaMask](https://metamask.io/)

## Tutorial

* Clone scaffold-eth's `simple-DAO-proposals` branch:
	```bash
	# this repo uses different branches for different projects
	# so if you want to explore it, look at the different branches
	# we use '--single-branch' to reduce the download/disk usage impact
	git clone https://github.com/scaffold-eth/scaffold-eth -b simple-DAO-proposals --single-branch

	cd scaffold-eth
	yarn install
	```

* Prepare Moonbeam

	This can take some time to download, so we'll kick that off now (e.g. in a new terminal).
	```bash
	docker pull purestake/moonbeam:v0.29.0
	```

* edit Hardhat conf (`packages/hardhat/hardhat.config.js`)
	* See [Hardhat's Config documentation](https://hardhat.org/hardhat-runner/docs/config) for more details
	* Add local moonbeam node as network in hardhat config
		```javascript
		// our local moonbeam node
		localMoonbeam: {
			url: "http://127.0.0.1:9933",
			gasPrice: 10000000000, // 10 gwei (10 billion)
			chainId: 1281,
			accounts: {
				mnemonic: mnemonic(),
			},
		},
		```
	* set `const defaultNetwork = "localMoonbeam";` (line 28)
	* set DEBUG = true (we want it to print out the `deployer` address) (line 311)

	
	* set `const mnemonic = "bottom drive obey lake curtain smoke basket hold race lonely fit walk";` (line 388)
	  This causes Hardhat to use the same accounts as the ones that are [pre-funded in a Moonbeam dev node](https://github.com/PureStake/moonbeam/#prefunded-development-addresses).
	  `TODO: find a proper way to integrate these accounts`

* Generate privkey
	```bash
	# run yarn generate to generate mnemonic for deployer. note that this is the "generate" task found in hardhat.config.js
	export NODE_OPTIONS=--openssl-legacy-provider # may need
	yarn generate
	# ensure that printed account matches Alith, or copy account if using different mnemonic
	```

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

* Edit `packages/hardhat/deploy/00_deploy_your_contract.js`
	* Add one or more keys from Moonbeam prefunded accounts
	* Replace addresses in `const members = [..]` (line 8)
	* E.g.:
	```javascript
	const members = [
		"0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac", // Alith
		"0x3Cd0A705a2DC65e5b1E1205896BaA2be8A07c6e0", // Baltathar
	];
	```

* Deploy
	```bash
	# chain needs to be running, so either:
	# have moonbeam node running (see above)
	# or we can run `yarn chain` in another terminal to run Hardhat's built in Ethereum node

	export NODE_OPTIONS=--openssl-legacy-provider # may need
	yarn deploy
	```

* Modify MetaMask to Add Network and Import Dev Accounts
	* Add Dev Network
		:warning: Matamask will complain if it can't connect to the RPC you specify, so you will need Moonbeam running\
		<img src="https://user-images.githubusercontent.com/2967426/221322347-8a424db9-2d91-414a-9782-3955670072dc.png" width="200"/>\
		<img src="https://user-images.githubusercontent.com/2967426/221322445-c7ef4972-24bf-4d0a-9773-384fa6da6ecf.png" width="350"/>\
		<img src="https://user-images.githubusercontent.com/2967426/221322383-a88b3760-6ea5-4b41-adf4-6c57d833edff.png" width="300"/>\
		
	* Add Dev Accounts\
		<img src="https://user-images.githubusercontent.com/2967426/221322466-e8316b57-f6ac-4061-b7cc-8527d8d38fa4.png" width="200"/>\
		<img src="https://user-images.githubusercontent.com/2967426/221322485-ea31516b-740b-469c-a0f6-075a12db7dbb.png" width="200"/>\

		* Alith: `0x5fb92d6e98884f76de468fa3f6278f8807c48bebc13595d45af5bdc4da702133`
		* Baltathar: `0x8075991ce870b93a8870eca0c0f91913d12f47948ca0fd25b49c6fa7cdbeee8b`
		* More: https://github.com/PureStake/moonbeam/#prefunded-development-addresses

* Interact

	At this point, we have our Moonbeam node running with a dev chain deployed.
	We also have the PowDAO contract deployed with Alith and Baltithar as initial members, and we have these accounts in MetaMask.
	Now we will deploy the frontend.

	Change port used for RPC (Moonbeam defaults to port 9933 rather than 8545):

	```bash
	# replace 8545 with 9933 in packages/react-app/src/constants.js

	# kicks off the web UI
	yarn start
	```

	Challenges:

	* Test membership
	* Add member
	* Process Proposal
	* Kick member
	* Request and receive a payout


## References

* [Moonbeam](https://moonbeam.network)
* [Polkadot](https://polkadot.network/)
* [Hardhat](https://hardhat.org/)
* [scaffold-eth](https://github.com/scaffold-eth/scaffold-eth)
* [MetaMask](https://metamask.io/)
