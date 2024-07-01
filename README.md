#  It is recommended to follow official docs to setup and run Aleo Aleo github repo.
#  https://github.com/AleoHQ/snarkOS#3a-run-an-aleo-client-node 
#  However, if you struggle to do it, proceed with this guide here.
#  We recommend Dedicated servers without the word ‘Virtual’. 32core or preferred 64 core Server with 500ssd minimum and 64gb RAM 


# aleo prover node 
Install aleo on any dedicated server

# Update
sudo apt update && sudo apt upgrade -y

# Install essential packages

sudo apt install wget jq git build-essential pkg-config libssl-dev -y

# install screen

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

# [For Ubuntu users] A helper script to install dependencies is available. From the snarkOS directory, run:  Lastly, install snarkOS:


./build_ubuntu.sh && cargo install --locked --path .

# check is snarkos is installed properly

snarkos --version

# Create New account & save the pvt key and its details safely you need in the next steps

snarkos account new

# run prover node

snarkos start --nodisplay --network 1 --prover --private-key=paste_savedpvtkey_here

# or u can also run this 

./run-prover.sh network 1

# to run client

snarkos start --nodisplay --network 1 --private-key=paste_savedpvtkey_here

# or

./run-client network 1

# To check scores for the incentive program, visit

https://testnetbeta.aleo123.io/address/enter_your_prover_address  # thanks @de441ck from discrod for this


 #  FAQs

 
1. My node is unable to compile.
Ensure your machine has Rust v1.66+ installed. Instructions to install Rust can be found here.
If large errors appear during compilation, try running cargo clean.
Ensure snarkOS is started using ./run-client.sh or ./run-prover.sh.
2. My node is unable to connect to peers on the network.
Ensure ports 4130/tcp and 3030/tcp are open on your router and OS firewall.
Ensure snarkOS is started using ./run-client.sh or ./run-prover.sh.
3. I can't generate a new address
Before running the command above (snarkos account new) try source ~/.bashrc
Also double-check the spelling of snarkos. Note the directory is /snarkOS, and the command is snarkos
4. How do I use the CLI to sign and verify a message?
Generate an account with snarkos account new if you haven't already
Sign a message with your private key using snarkos account sign --raw -m "Message" --private-key-file=<PRIVATE_KEY_FILE>
Verify your signature with snarkos account verify --raw -m "Message" -s sign1SignatureHere -a aleo1YourAccountAddress
Note, using the --raw flag with the command will sign plaintext messages as bytes rather than Aleo values such as 1u8 or 100field.

5. Command Line Interface
To run a node with custom settings, refer to the options and flags available in the snarkOS CLI.

# The full list of CLI flags and options can be viewed with snarkos --help:

snarkOS 

The Aleo Team <hello@aleo.org>

USAGE:
    snarkos [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help                     Print help information
    -v, --verbosity <VERBOSITY>    Specify the verbosity [options: 0, 1, 2, 3] [default: 2]

SUBCOMMANDS:
    account    Commands to manage Aleo accounts
    clean      Cleans the snarkOS node storage
    help       Print this message or the help of the given subcommand(s)
    start      Starts the snarkOS node
    update     Update snarkOS
The following are the options for the snarkos start command:

USAGE:
    snarkos start [OPTIONS]

OPTIONS:
        --network <NETWORK_ID>                  Specify the network ID of this node [default: 3]
        
        --validator                             Specify this node as a validator
        --prover                                Specify this node as a prover
        --client                                Specify this node as a client
        
        --private-key <PRIVATE_KEY>             Specify the node's account private key
        --private-key-file <PRIVATE_KEY_FILE>   Specify the path to a file containing the node's account private key
        
        --node <IP:PORT>                        Specify the IP address and port for the node server [default: 0.0.0.0:4130]
        --connect <IP:PORT>                     Specify the IP address and port of a peer to connect to
 
        --rest <REST>                           Specify the IP address and port for the REST server [default: 0.0.0.0:3030]
        --norest                                If the flag is set, the node will not initialize the REST server
        
        --nodisplay                             If the flag is set, the node will not render the display
        --verbosity <VERBOSITY_LEVEL>           Specify the verbosity of the node [options: 0, 1, 2, 3] [default: 2]
        --logfile <PATH>                        Specify the path to the file where logs will be stored [default: /tmp/snarkos.log]
        
        --dev <NODE_ID>                         Enables development mode, specify a unique ID for this node
6. Development Guide
6.1 Quick Start
In the first terminal, start the first validator by running:

cargo run --release -- start --nodisplay --dev 0 --validator
In the second terminal, start the second validator by running:

cargo run --release -- start --nodisplay --dev 1 --validator
In the third terminal, start the third validator by running:

cargo run --release -- start --nodisplay --dev 2 --validator
In the fourth terminal, start the fourth validator by running:

cargo run --release -- start --nodisplay --dev 3 --validator
From here, this procedure can be used to further start-up provers and clients.

6.2 Operations
It is important to initialize the nodes starting from 0 and incrementing by 1 for each new node.

The following is a list of options to initialize a node (replace <NODE_ID> with a number starting from 0):

cargo run --release -- start --nodisplay --dev <NODE_ID> --validator

cargo run --release -- start --nodisplay --dev <NODE_ID> --prover

cargo run --release -- start --nodisplay --dev <NODE_ID> --client

cargo run --release -- start --nodisplay --dev <NODE_ID>

**When no node type is specified, the node will default to --client.**
