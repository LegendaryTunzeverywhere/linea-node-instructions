# linea-node-instructions
contains instructions on how to run linea node

Open Ubuntu
# Run this
```
mkdir linea && cd linea
```

# Install stable version of Geth -- run the below code individually
```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

# To upgrade existing Geth
```
sudo apt-get upgrade geth
```

# Run geth and allow it pass through your firewall
run 
```
geth
```
(press enter)
press `ctrl + c` to cancel and resume terminal prompt

# create genesis.json file
```
vi genesis.json
```
(press enter)
press i

** copy and paste text from this ![Link](https://docs.linea.build/files/genesis.json) into the buffer
press esc, :x (press enter)

# Next step is to create space(volume) for your node 

use `df -h` to check available space, focus on the C:/
if you have more than 100GB. allocate it for the purpose, `fallocate -l 100G myfile.img`

in my case I'm using 5GB only `fallocate -l 5G myfile.img`

then format the file using this `mkfs.ext4 myfile.img`
wait for it to be successful, then proceed to mount it

```
mount -o loop myfile.img /mnt/myvolume
```

# Bootstrap the node using 
```
geth --datadir ./geth-linea-data init ./genesis.json
```

After success

#  put in command in a shell file

copy this 👇👇
```
#!/bin/bash/sh
geth \
--datadir $HOME/geth-linea-data \
--networkid 59144 \
--rpc.allow-unprotected-txs \
--txpool.accountqueue 50000 \
--txpool.globalqueue 50000 \
--txpool.globalslots 50000 \
--txpool.pricelimit 1000000 \
--txpool.pricebump 1 \
--txpool.nolocals \
--http --http.addr '127.0.0.1' --http.port 8545 --http.corsdomain '*' --http.api 'web3,eth,txpool,net' --http.vhosts='*' \
--ws --ws.addr '127.0.0.1' --ws.port 8546 --ws.origins '*' --ws.api 'web3,eth,txpool,net' \
--bootnodes "enode://ca2f06aa93728e2883ff02b0c2076329e475fe667a48035b4f77711ea41a73cf6cb2ff232804c49538ad77794185d83295b57ddd2be79eefc50a9dd5c48bbb2e@3.23.106.165:30303,enode://eef91d714494a1ceb6e06e5ce96fe5d7d25d3701b2d2e68c042b33d5fa0e4bf134116e06947b3f40b0f22db08f104504dd2e5c790d8bcbb6bfb1b7f4f85313ec@3.133.179.213:30303,enode://cfd472842582c422c7c98b0f2d04c6bf21d1afb2c767f72b032f7ea89c03a7abdaf4855b7cb2dc9ae7509836064ba8d817572cf7421ba106ac87857836fa1d1b@3.145.12.13:30303" \
--syncmode full \
--metrics \
--verbosity 3
```

# and create a file called run.sh
```
vi run.sh
```
press `i` and paste the above code into it.

press esc, :x (press enter)

# make it an executable
chmod +x run.sh

#run the shell script with 
```
./run.sh
```
