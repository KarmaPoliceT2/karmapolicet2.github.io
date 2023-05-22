# RocketPool Setup

## Creating the node wallet

1. Run `rocketpool wallet init`

    - Proceed only if you're in a secure location without prying eyes
    - Enter a complex password to secure the wallet
    - Confirm the password
    - Copy your mnemonic to a secure location
    - Enter the number of words in your mnemonic
    - Enter the mnemonic one word at a time

## Seed the node wallet

1. This account will be primarily responsible for gas fees incurred by the node, 0.1 ETH should be more than enough to get started.

    - Run `rocketpool node status` to get your node wallet account address
    - Transfer 0.1 ETH from metamask to the node wallet
    - Result:
      > :exclamation: [Transaction](https://goerli.etherscan.io/tx/0xb05152bf1beda8f699de3d6f403b64eaa12d20f80dc5c27819cc9b43ab9ef5a1)

## Register the node

1. In order for RocketPool to know your node exists, it must be registered

    - Run `rocketpool node register`
    - Allow it to detect your timezone automatically (it should detect correctly if you set it up as described in the os installation section)
    - Accept the timezone if it is correct
    - Enter your desired max fee (or accept the sensible defaults)
    - Result:
      > :exclamation: [Transaction](https://goerli.etherscan.io/tx/0x0aee0cdf616c303b932d466b3ea0c6684cbe93fb406c87206c6f1f4ed3036aa0)

## Setup your withdrawal account

1. This should DEFINITELY be on your hardware wallet, and should have at least a minor amount of ETH in it to support a transaction fee during the initial setup.

2. Set your address using the command `rocketpool node set-withdrawal-address < your cold wallet address >`

    - Enter your desired max fee (or accept the sensible defaults)
    - Confirm your address is entered correctly
    - Result:
      > :exclamation: [Transaction](https://goerli.etherscan.io/tx/0x69f16505a9322fbb4abf6ee1439061c310bb8e9b5656be0346ddd4d22b6f1229)

3. The withdrawal address status is now pending, you need to confirm it via the RocketPool website [Mainnet here](https://stake.rocketpool.net/withdrawal/)

    > :exclamation: [Testnet here](https://testnet.rocketpool.net/withdrawal/)
    - Copy your node address into the site
    - Click 'Connect Wallet'
    - Accept the Terms & Conditions
    - Select your Withdrawal address from the MM list
        - If you don't get a list it's because one of your other addresses is already connected to RocketPool, go through the "connected sites" list on your addresses and disconnect anything connected to RocketPool
    - Click 'Find'
    - Confirm the address you see is in fact the correct withdrawal address
    - Click 'Confirm Pending'
    - Accept the transaction on your hardware wallet

4. Confirm the withdrawal address is updated on the node with `rocketpool node status`

## Set your voting delegate

1. If you want to vote yourself, follow these instructions:

    - `rocketpool node set-voting-delegate < voting delegate address >`
    - Enter your desired max fee (or accept the sensible defaults)
    - Confirm the address you entered is correct
    - Result:
      > :exclamation: [Transaction](https://goerli.etherscan.io/tx/0xbb68724a06cd2625b686e76b78fed510dfcdbec54affc9c67651bafdecc56a1f)

2. If you want to delegate your vote to someone else, set this setting to their address instead of your own.

## Join the Smoothing Pool

1. To participate in more regular but shared rewards join the smoothing pool via the command `rocketpool node join-smoothing-pool`

    - Enter your desired max fee (or accept the sensible defaults)
    - Confirm that you want to join the smoothing pool
    - Result:
      > :exclamation: [Transaction](https://goerli.etherscan.io/tx/0x9a3cb5c80609bc3758a4be1e93d969b25d2e119e5ec89b79628d53426d243801)
