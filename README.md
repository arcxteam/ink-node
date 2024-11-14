### ink-node
A Complete Guide - Run Node Ink L2 Blockchain Built on OP Superchain by Kraken

The Future of Ink blockchain for DeFi applications built on Optimism Superchain and released by Kraken



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

op-node API 🌐
You can use the optimism_syncStatus method on the op-node API to know what’s the current status:

```
curl -X POST -H "Content-Type: application/json" --data \
    '{"jsonrpc":"2.0","method":"optimism_syncStatus","params":[],"id":1}' \
    http://localhost:9545 | jq
```


op-geth API 🌐
When your local node is fully synced, calling the eth_blockNumber method on the op-geth API should return the latest block number as seen on the block explorer.

```
curl http://localhost:8545 -X POST \
    -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params": [],"id":1}' | jq -r .result | sed 's/^0x//' | awk '{printf "%d\n", "0x" $0}';
```

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




