# A Complete Guide - Run Node Ink, the L2 Blockchain Built-on OP Superchain by Kraken

Ink is Layer 2 blockchain project focussing for DeFi applications that built-on Optimism Superchain and released by from the team behind Kraken (Payward Inc)

## Here We Go...Gasss!!

**`This incentivized, incentive....?`**

> *Following saying not have incentive testnet, does familiar see other L2 projected. BUT at now many L2 tokenomics are released.*

**FOR HUGE UPDATES, WILL BE SHARING AT THIS REPO**

---

## 1. Preparation for Ink Node
**1. Hardware Requirements** 

`In order to run Ink node, its need a server like VPS with the minimum recommended hardware`
| Requirement                      | Details                                   |
|----------------------------------|-------------------------------------------|
| RAM/Memory                       | 6 - 16 GB                                 |
| CPU/vCPU                         | 6 - 8 Cores                               |
| Storage Space                    | 100 GB - More                             |
| Supported OS                     | Ubuntu 20.04-22.04-24.04 LTS              |

*Running an archive node will demand additional disk space over time. More CPU and RAM will enhance performance with higher RPC traffic.*

**2. Software Prerequisites**

`In order to run Ink node, its need ensure Docker is installed on your server (VPS)`
| Requirement                  | Details                                   |
|------------------------------|-------------------------------------------|
| Docker                       | [Docker Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) |
| Installation Script          | Monitor Based-on Docker                   |

**3. Rollup-specific Funct as Consensus-Layer**

`Sepolia L1 RPC & Beacon APIs (API specification for the beacon chain, RPC endpoints)`
- ETH Sepolia L1 RPC public or private [Get RPC](https://www.google.com/search?q=get+sepolia+eth+RPC&oq=get+sepolia+eth+RPC&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRigATIHCAIQIRigATIHCAMQIRigATIHCAQQIRigATIHCAUQIRifBTIHCAYQIRifBTIHCAcQIRifBTIHCAgQIRiPAjIHCAkQIRiPAtIBCDk2NzlqMGo3qAIIsAIB&sourceid=chrome&ie=UTF-8)
- ETH Beacon L1 RPC public or private [Get RPC](https://www.google.com/search?q=get+beacon+sepolia+eth+RPC&oq=get+beacon+sepolia+eth+RPC&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABiABBiiBDIKCAIQABiABBiiBDIKCAMQABiABBiiBDIKCAQQABiiBBiJBTIKCAUQABiABBiiBNIBCTE1MzFqMGoxNagCCLACAQ&sourceid=chrome&ie=UTF-8)

**4. Port on The Firewall Server (optional)**

`Binding or not its optional, but you need know this port are open for tcp/udp`

- node-op-node: on the host uses PORT 6060, 7300, 9222, 9545
- node-op-gEth: on the host uses PORT 8545, 8546, 30303, 7301
> check your port use it or not `sudo ufw status`

## 2. Running Ink Node Installation
**1. Installer bind (port) firewall configured**
```sh
sudo ufw allow 9999 && sudo ufw allow 8900 && sudo ufw allow 9000 && sudo ufw allow 3030 && sudo ufw reload
```

**2. Run OriginTrail V8 DKG core node installer** 
```sh
cd /root/ && curl -k -o v8_installer.sh https://raw.githubusercontent.com/OriginTrail/ot-node/v8/develop/installer/v8_installer.sh && chmod +x v8_installer.sh
```

**3. Execute the installer by running**
```sh
./v8_installer.sh
```

**4. Verify V8 DKG Core node installation**

- During node installation process stay cool and read on ssh logs, maybe your menu like image below. this OK status


- If your installation has been successful, your node will show the `Node is up and running!` log as shown in the example image. **Congratulations, your V8 DKG Core node is up and running!**



## 3. Checking, Leaderboard and Usefull Commands
**1. Super cmd logs**

- Start node: `systemctl start otnode`
- Stop node: `systemctl stop otnode`
- Restart node: `systemctl restart otnode`
- Show node logs: `journalctl -u otnode --output cat -fn 100`
- Open node config: `nano /root/ot-node/.origintrail_noderc`
- Save and backup: `cat /root/ot-node/.origintrail_noderc`















---

node-op-node: Menggunakan port 6060, 7300, 9222, dan 9545 pada host.
node-op-geth: Menggunakan port 8545, 8546, 30303, dan 7301 pada host.


### ink-node

Ink OG
to buy on OP Ethereum equal $0.5 cent
Hold the celebratory Ink launch NFT!

https://guild.xyz/inkonchain 


Download the Ink Git repository & Go to Ink Directory

```
git clone https://github.com/inkonchain/node
cd node
```

Edit a .env.ink-sepolia file

```
nano .env.ink-sepolia
```
2 lines in the .env file edit
```
L1_RPC_URL=https://ethereum-sepolia-rpc.publicnode.com
L1_BEACON_URL=https://ethereum-sepolia-beacon-api.publicnode.com
```
Save your .env by pressing CTRL+X write Y and press Enter

Installation 📥
Run the setup script:
```
./setup.sh
```
Do you want to fetch the latest snapshot? [Y/N]: y

Execution 🚀
Start the Ink node using Docker Compose:
```
docker compose up -d
```

Save your private key

```
cat var/secrets/jwt.txt
```

## Verifying Sync Status 🔎

Using op-node API 🌐

You can use the optimism_syncStatus method on the op-node API to know what’s the current status: 

Check the sync status using the optimism_syncStatus method:

```
curl -X POST -H "Content-Type: application/json" --data \
    '{"jsonrpc":"2.0","method":"optimism_syncStatus","params":[],"id":1}' \
    http://localhost:9545 | jq
```

Using op-geth API 🌐
To verify if your node is fully synced, use the eth_blockNumber method:

When your local node is fully synced, calling the eth_blockNumber method on the op-geth API should return the latest block number as seen on the block explorer.

```
curl http://localhost:8545 -X POST \
    -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params": [],"id":1}' | jq -r .result | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}';
```

A synced node will display the latest block number as seen on the block explorer.

Comparing with Ink Public RPC
Compare your local node's finalized block with the public RPC block:

Comparing w/ Remote RPC 👀
Use this script to compare your local finalized block with the one retrieved from the Remote RPC:

```
local_block=$(curl -s -X POST http://localhost:8545 -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["finalized", false],"id":1}' \
  | jq -r .result.number | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}'); \
remote_block=$(curl -s -X POST https://rpc-gel-sepolia.inkonchain.com/ -H "Content-Type: application/json" \
 --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["finalized", false],"id":1}' \
 | jq -r .result.number | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}'); \
echo -e "Local finalized block: $local_block\nRemote finalized block: $remote_block"
```


The node is in sync when both the Local finalized block and Remote finalized block are equal. E.g.:
Check the lastest block return by your node and compare with explorer on
https://explorer-sepolia.inkonchain.com/

```
Local finalized block: 4449608
Remote finalized block: 4449608
```

1 . ![inknode2](https://github.com/user-attachments/assets/a77a24ce-a757-49fe-b7dc-e901886dfa40)

2. ![inknode3](https://github.com/user-attachments/assets/16e25d4d-3a36-41bb-92b2-192030965f66)

3. ![inknode1](https://github.com/user-attachments/assets/007f3910-837f-4d80-9183-70da76c1ad39)

4. ![inknode4](https://github.com/user-attachments/assets/b40f71c8-01a2-4578-bdce-8ed713a883c5)


**3. Important note** 
note: Syncing your node may take several days. Monitor usage and plan accordingly.

