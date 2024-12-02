# A Complete Guide - Run Ink Node, The L2 Blockchain Built-on Optimism Superchain by Kraken

![Testnet Node - Full Guides cover](https://github.com/user-attachments/assets/67c8b448-ff70-4697-b55a-8442cd90a7dd)

Ink is a Layer 2 blockchain project specifically tailored for DeFi applications built on the Optimism Superchain (OP Stack). Ink by the team behind Kraken, officially known as Payward Inc.

## Here We Go...GAS 

**`Is there incentivized?` ![Confirm](https://img.shields.io/badge/confirm-not_yet-brightgreen)**

> [!IMPORTANT]
> currently, there isn't an incentive-basis testnet for Ink, it's worth noting that many Layer 2 projects to release tokenomics and incentives over time. Following these projects can provide insights into potential incentives and benefits down the line, as we’ve seen with other successful L2. **FOR HUGE UPDATES, WILL BE SHARING AT THIS REPO**

---

## 1. Preparation - Run Ink Node
**1. Hardware Requirements**

`In order to run Ink node, its need a server like VPS with the minimum recommended specs`
| Requirement                      | Details                                   |
|----------------------------------|-------------------------------------------|
| RAM/Memory                       | 6 - 16 GB                                 |
| CPU/vCPU                         | 6 - 8 Cores                               |
| Storage Space                    | 100 GB - More                             |
| Supported OS                     | Ubuntu 20.04-22.04-24.04 LTS              |

*Running an archive node will demand additional disk space over time. More CPU and RAM will enhance performance with higher RPC traffic.*

![inknode3](https://github.com/user-attachments/assets/16e25d4d-3a36-41bb-92b2-192030965f66)

**2. Software Prerequisites**

`In order to run Ink node, its need ensure Docker is installed on your server (VPS)`
| Requirement                  | Details                                   |
|------------------------------|-------------------------------------------|
| Docker                       | [Docker Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) |
| Installation Script          | Monitor Based-on Docker                   |

> `Not yet Docker install? trying auto installing...`
```
curl sSL https://raw.githubusercontent.com/arcxteam/glacier-node/main/install-docker.sh | bash
```

**3. Rollup-specific Funct as Consensus-Layer** 

`Sepolia L1 RPC & Beacon APIs (API specification for the beacon chain, RPC endpoints)`
- ETH Sepolia L1 RPC public or private [Get RPC](https://www.google.com/search?q=get+sepolia+eth+RPC&oq=get+sepolia+eth+RPC&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRigATIHCAIQIRigATIHCAMQIRigATIHCAQQIRigATIHCAUQIRifBTIHCAYQIRifBTIHCAcQIRifBTIHCAgQIRiPAjIHCAkQIRiPAtIBCDk2NzlqMGo3qAIIsAIB&sourceid=chrome&ie=UTF-8)
- ETH Beacon L1 RPC public or private [Get RPC](https://www.google.com/search?q=get+beacon+sepolia+eth+RPC&oq=get+beacon+sepolia+eth+RPC&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABiABBiiBDIKCAIQABiABBiiBDIKCAMQABiABBiiBDIKCAQQABiiBBiJBTIKCAUQABiABBiiBNIBCTE1MzFqMGoxNagCCLACAQ&sourceid=chrome&ie=UTF-8)

**4. Port on The Firewall Server (optional)**

`Binding or not its optional, but you need know this port are open for tcp/udp`

- node-op-node: on the host uses PORT 6060, 7300, 9222, 9545
- node-op-gEth: on the host uses PORT 8545, 8546, 30303, 7301
> note; checking other port use it or not, w/ cmd> `sudo ufw status`

## 2. Installation - Run Ink Node
**1. Configured Binding Port (optional)**
```sh
sudo ufw allow 6060/tcp && sudo ufw allow 7300/tcp && sudo ufw allow 9222/tcp && sudo ufw allow 9545/tcp && sudo ufw allow 9222/udp && sudo ufw allow 8545:8546/tcp && sudo ufw allow 30303/tcp && sudo ufw allow 30303/udp && sudo ufw allow 7301/tcp && sudo ufw reload
```

**2. Clone Repository**
```
git clone https://github.com/inkonchain/node
cd node
```

**3. Edit File Name env.ink-sepolia**
```
nano .env.ink-sepolia
```
- In both lines, replace the RPC or use this

- Save and exit the editor (press `CTRL+X`, then `Y`, and `ENTER`)

```diff
- Example:
L1_RPC_URL=https://sepolia.drpc.org
L1_BEACON_URL=https://eth-beacon-chain-sepolia.drpc.org/rest/
```
![inknode1](https://github.com/user-attachments/assets/007f3910-837f-4d80-9183-70da76c1ad39)

**4. Run the Setup Script**

`Run at this`
```
./setup.sh
```
![inknode2](https://github.com/user-attachments/assets/a77a24ce-a757-49fe-b7dc-e901886dfa40)

- stay calm... processing a 17GB file & setup underway. Plz check back in a few minutes...an estimate around **10-15 minutes** for the setup. Once complete, run the docker command below.

`Start execution on Docker`
```
docker compose up -d
```

`Save your private key`
```
cat var/secrets/jwt.txt
```

## 3. Verifying Sync & Run Status 
**1. Using the op-node API**

- Check the sync status with the `optimism_syncStatus` method on the op-node API to monitor your node’s current status.
```
curl -X POST -H "Content-Type: application/json" --data \
    '{"jsonrpc":"2.0","method":"optimism_syncStatus","params":[],"id":1}' \
    http://localhost:9545 | jq
```

**2. Using the op-geth API**

- Verify full sync by calling the `eth_blockNumber` method on the op-geth API. If synced, this should match the latest block number on a block explorer.`
```
curl http://localhost:8545 -X POST \
    -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params": [],"id":1}' | jq -r .result | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}';
```

**3. Compare with Remote RPC**

- Use this commands to compare your local finalized block against the finalized block from the Remote RPC.
```
local_block=$(curl -s -X POST http://localhost:8545 -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["finalized", false],"id":1}' \
  | jq -r .result.number | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}'); \
remote_block=$(curl -s -X POST https://rpc-gel-sepolia.inkonchain.com/ -H "Content-Type: application/json" \
 --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["finalized", false],"id":1}' \
 | jq -r .result.number | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}'); \
echo -e "Local finalized block: $local_block\nRemote finalized block: $remote_block"
```

- The node is in sync when both the Local finalized block and Remote finalized block are equal. Check the lastest block return by your node and compare with explorer on https://explorer-sepolia.inkonchain.com/

```diff
- Results logs --> Local finalized block: 4449608
- Result logs --> Remote finalized block: 4449608
```
![inknode4](https://github.com/user-attachments/assets/b40f71c8-01a2-4578-bdce-8ed713a883c5)

> [!NOTE]
> **Important** A syncing your node may take several days. Monitor usage and plan accordingly.

## 4. Super Commands Logs

- Save private key cmd.> `cat var/secrets/jwt.txt`
- Logs info node-op-node cmd.> `docker logs -f node-op-node-1 --tail=10`
- Logs info node-op-geth cmd.> `docker logs -f node-op-geth-1 --tail=10`
- Logs status resources cmd.> `docker stats`

## 5. Get Ink OG Discord

- Join to Ink Discord [![Discord](https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com/invite/inkonchain)
- Verified Member
- Get role https://guild.xyz/inkonchain 
- Ink Apprentice Dev, Ink Novice Dev
- Hold the celebratory Ink launch NFT!
- To buy NFT on OP Ethereum equal $0.5 cent [Introducing Ink. Built on the Superchain ✨](https://zora.co/collect/oeth:0x5d1e1a5cdd95f68ff18d78242c252f6ceaa4538b/2?referrer=0xbF149aAB2640967BD4685B305A05f1e3EE6ce38b)
