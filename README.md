

<div align=center>
  <img src="https://github.com/TempGROX/TempGROX/blob/main/src/photos/MorpLogo_200x200.jpg">
  <h1 align=center>Install-morph-validator-node</h1>
  <p>This repository describes in detail how to install the validator node of the Morph project.</p>
  <br>
</div>

# Build From Source Code
## Clone Morph
```bash
mkdir -p ~/.morph 
cd ~/.morph
git clone https://github.com/morph-l2/morph.git
```
Now we move on to the current branch. At the time of writing the guide, this is a branch: v0.1.0-beta
```bash
cd morph
git checkout v0.1.0-beta
```
## Build Geth
```bash
make nccc_geth
```
## Build Node
```bash
cd ~/.morph/morph/node 
make build
```

# Sync from the genesis block
## Download genesis file
```bash
cd ~/.morph
wget https://raw.githubusercontent.com/morph-l2/config-template/main/holesky/data.zip
unzip data.zip
```
Create a secret
```bash
cd ~/.morph
openssl rand -hex 32 > jwt-secret.txt
```

# Start two process
## Geth
```bash
NETWORK_ID=2810

nohup ./morph/go-ethereum/build/bin/geth \
--datadir=./geth-data \
--verbosity=3 \
--http \
--http.corsdomain="*" \
--http.vhosts="*" \
--http.addr=0.0.0.0 \
--http.port=8545 \
--http.api=web3,eth,txpool,net,engine \
--ws \
--ws.addr=0.0.0.0 \
--ws.port=8546 \
--ws.origins="*" \
--ws.api=web3,eth,txpool,net,engine \
--networkid=$NETWORK_ID \
--authrpc.addr="0.0.0.0" \
--authrpc.port="8551" \
--authrpc.vhosts="*" \
--authrpc.jwtsecret=$JWT_SECRET_PATH \
--gcmode=archive \
--metrics \
--metrics.addr=0.0.0.0 \
--metrics.port=6060 \
--miner.gasprice="100000000"
```

## Node

```bash
cd ~/.morph
export L1MessageQueueWithGasPriceOracle=0x778d1d9a4d8b6b9ade36d967a9ac19455ec3fd0b
export START_HEIGHT=1434640
export Rollup=0xd8c5c541d56f59d65cf775de928ccf4a47d4985c
./morph/node/build/bin/morphnode --validator --home ./node-data \
     --l2.jwt-secret ./jwt-secret.txt \
     --l2.eth http://localhost:8545 \
     --l2.engine http://localhost:8551 \
     --l1.rpc $(Ethereum Holesky RPC)  \
     --l1.beaconrpc $(Ethereum Holesky beacon chain RPC)  \
     --l1.chain-id  17000   \
     --validator.privateKey $(Your Validator Key)  \
     --sync.depositContractAddr $(L1MessageQueueWithGasPriceOracle) \
     --sync.startHeight  $(START_HEIGHT) \
     --derivation.rollupAddress $(Rollup) \
     --derivation.startHeight  $(START_HEIGHT) \
     --derivation.fetchBlockRange 200 \
     --log.filename ./node.log
```

# The end!
If you liked this guide, please go to all my social networks)

<br>
<div id="badges" align="center">
  <a href="https://discord.com/users/961408999411048461">
    <img src="https://img.shields.io/badge/Discord-blue?style=for-the-badge&logo=https%3A%2F%2Fimg.icons8.com%2Fios%2F50%2Fmedium-logo.png&logoColor=white" alt="LinkedIn Badge"/>
  </a>
  <a href="https://medium.com/@James_Brandon">
    <img src="https://img.shields.io/badge/Medium-black?style=for-the-badge&logo=https%3A%2F%2Fimg.icons8.com%2Fios%2F50%2Fmedium-logo.png&logoColor=white" alt="Youtube Badge"/>
  </a>
  <a href="https://keybase.io/jamesbrandon">
    <img src="https://img.shields.io/badge/Keybase-orange?style=for-the-badge&logo=https%3A%2F%2Fimg.icons8.com%2Fios%2F50%2Fmedium-logo.png&logoColor=white">
  </a>
  <a href="https://x.com/JBTGrox">
    <img src="https://img.shields.io/badge/Twitter-blue?style=for-the-badge&logo=twitter&logoColor=white" alt="Twitter Badge"/>
  </a>
  <a href="https://linktr.ee/JBrandon_?utm_source=linktree_admin_share">
    <img src="https://img.shields.io/badge/Linktree-green?style=for-the-badge&logo=https%3A%2F%2Fimg.icons8.com%2Fios%2F50%2Fmedium-logo.png&logoColor=white">
  </a>
</div>
