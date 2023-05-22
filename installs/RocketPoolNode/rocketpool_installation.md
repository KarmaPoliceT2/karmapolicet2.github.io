# RocketPool Installation

## Assumptions

- There are several ways to run a rocketpool node, for simplicity and maintenance we've chosen the docker based process here. Having tried native mode, it's not easy to get it integrated with some of the assumptions in the rocketpool installation process. There's also a hybrid mode but because we're only running a single node system here there's been no need for us to try this method. Of course you can choose your own method, but you'll want to follow a different guide if not choosing the docker method.

- The paths used below aren't absolutely required, but you'll have to do some significant tweaking for anything that's not explicitly a configurable path.

## Install the CLI

- Setup a directory for the software, download it, and make it executable

    ```text
    mkdir -p ~/bin
    wget https://github.com/rocket-pool/smartnode-install/releases/latest/download/rocketpool-cli-linux-amd64 -O ~/bin/rocketpool
    chmod +x ~/bin/rocketpool
    ```

- Exit your SSH session and start a new one so that the path created above is included in your PATH variable. There are other more graceful ways to do this, but logout/in is probably the easiest for everyone to understand.

- Confirm the CLI is now available by running `rocketpool --version`

## Install Smartnode

- This command does a lot of heavy lifting, it installs basically the entire node software, after this completes, we'll configure it:

    ```text
    rocketpool service install
    ```

- Once again exit your SSH session and start a new one so that everything is accessible through your path

## Configure the node

- The first time you run this it will bring up a wizard to follow, subsequent times will require editing settings more explicitly.

    ```text
    rocketpool service config
    ```

    1. **Welcome Screen**: Next
    2. **Network**: Ethereum Mainnet
        > :exclamation: 2. **Network**: Prater Testnet
    3. **Client Mode**: Locally Managed
    4. **Execution Client**: < Your Choice > (We use Besu)
    5. **Consensus Client**: < Your Choice > (We use Teku)
    6. **Custom Graffiti**: < Your Choice > (We use 'HeTalksInMaths')
    7. **Checkpoint Sync**: If you use a consensus client that supports it, enter a url here (Teku does support this), [list of URLs](https://eth-clients.github.io/checkpoint-sync-endpoints/#goerli). Make sure you use a URL for your network choice.
    8. **Fallback Clients**: If you have them, enter them, most wont
    9. **Metrics**: If you want to setup the grafana dashboard (highly recommended) then answer Yes to this
    10. **MEV Boost**: Locally Managed
    11. **MEV Boost Profile**: Select whichever you prefer
    12. **Finished**: Review if you'd like, or save and exit to get running
    13. **Start automatically**: Yes
    14. **WARNING**: Read it, but almost certainly yes

- Because this is a new install, you don't have to wait the requisite 15mins to start the node, so run the command:

    ```text
    rocketpool service start --ignore-slash-timer
    ```

- Confirm the system is on the network and versions you want:

    ```text
    rocketpool service version
    ```

- Check the services are running, run the following command a couple of seconds apart, checking for any "Restarting..." messages in the status column.

    ```text
    docker ps
    ```

- You now need to wait for the execution and consensus clients to sync to the network. To check the sync status of the node run the following command until you see 'fully synced'. ***This can take a long while.***

   ```text
   rocketpool node sync
   ```

  Prater TestNet took xxx mins when started on 9:29pm 5/19 ended on 5:55am 5/20
