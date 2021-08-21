# Running a Prysm client for SBC

This document describes how to run Prysm beacon node + Prysm validator for the Stake Beacon Chain.

## Assumptions
* This document assumes that you already have an xDai node available for your use (or public JSON RPC endpoint)
* You have already generated your validator accounts using the fork of the official deposit-cli - https://github.com/canarynetwork/eth2.0-deposit-cli. You will need validator keystores and passwords for them to run the validator client.
* You might start your node and validator first, and only them make a deposit, once everything works.
* You have a persistent linux VM with docker installed on it, which is accessible from the public internet via a fixed IP address. We recommend using a VM with at least 4 vCPU, 8 GB RAM, 100 GB SSD

## Configuration
1) Create an empty directory somewhere on the VM (e.g. `/root/sbc`)
2) Clone the repository with all necessary configs:
```bash
cd /root/sbc
git clone https://github.com/canarynetwork/sbc-prysm .
```
3) Generate wallet password and place it in the `./keys/wallet_password.txt`
4) Copy all validators keystore files to the `./keys/validator_keys` directory. Ensure that copied keystores are only used on a single VM instance.
5) Write keystore password to the `./keys/keystore_password.txt` file
6) Create `.env` file from the example at `.env.example`. Put the valid external IP address of your VM and xDAI RPC url in the config. Other values can be left without changes.

## Import of validator keys
1) Run the following command to import and all added keystore files:
```bash
docker-compose up validator-import
```
2) If you want to confirm that all validators are imported correctly, you can run the following command to see the whole list:
```bash
docker-compose up validator-list
```

## Running node
1) Run the following command to start the beacon node:
```bash
docker-compose up -d node
docker-compose logs -f node
```
2) Your node should find other peers and start syncing with the rest of the network

## Running validator
1) Run the following command to start the validator client:
```bash
docker-compose up -d validator
docker-compose logs -f validator
```
