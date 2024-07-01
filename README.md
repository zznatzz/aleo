# aleo prover node 
Install aleo on any dedicated server

# Update
sudo apt update && sudo apt upgrade -y

**#Install essential packages**
sudo apt install wget jq git build-essential pkg-config libssl-dev -y

**#install screen**
sudo apt install screen
screen -S aleo
# to come out of acreen ctl a d and to join back to the screen -- use --> screen -r aleo

# Install rust
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/rust.sh)

source "$HOME/.cargo/env"

# Please ensure ports 4130/tcp and 3030/tcp are open on your router and OS firewall.

sudo ufw allow 3030/tcp
sudo ufw allow 4130/tcp

# Start by cloning this GitHub repository:

git clone --branch mainnet --single-branch https://github.com/AleoNet/snarkOS.git
# Next, move into the snarkOS directory:

cd snarkOS
git checkout tags/testnet-beta

# [For Ubuntu users] A helper script to install dependencies is available. From the snarkOS directory, run:
# Lastly, install snarkOS:

./build_ubuntu.sh && cargo install --locked --path .

**#check is snarkos is installed properly**
snarkos --version

# Create New account & save the pvt key and its details safely
snarkos account new

**#run prover node**

snarkos start --nodisplay --network 1 --prover --private-key=paste_savedpvtkey_here

# or u can also run this 

./run-prover.sh network 1

**#to run client**
snarkos start --nodisplay --network 1 --private-key=paste_savedpvtkey_here
or
./run-client network 1
