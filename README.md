Run a Light Node with One Command
Linux/Mac
Windows
To start your light node, simply run the following command:

Copy
curl -sL1 https://nubit.sh | bash
Whether you are launching the light node for the first time or are an existing user, this single command will automatically upgrade to the latest version of nubit-node and initiate the light node, regardless of the directory you are in
Run a Light Node in the Background Continuously
You can run a node in the background so you don't need to keep your terminal open all the time. Corresponding you also need to stop the process manually if you want to stop the node in the future. 
Below is the detailed instructions:
We recommend command nohup for beginners. Other means like tmux, systemctl are only recommended for practiced hands.
Starting a Node in Background
Please open a terminal & type the following command to start node service in the background:
Copy
nohup curl https://nubit.sh | bash > $HOME/nubit-light.log 2>&1 &
Then check the latest log by the following command to see whether some error occurs. You can also check the log later at any time.
Copy
tail -n 20 $HOME/nubit-light.log
You can also check whether the node has been started successfully, after a minute, by typing in:
Copy
lsof -i :2121 | grep LISTEN
and you would see outputs similar to this if the node has been started successfully:
Copy
nubit   1731890 root  410u  IPv6 69875760      0t0  TCP *:scientia-ssdb (LISTEN)
nubit   1731890 root  412u  IPv4 69875759      0t0  TCP *:scientia-ssdb (LISTEN)
If nothing shows, the node starting up fails.
Stopping a Background-running Node
Please open a new terminal first. Type in the following command to see whether there is a node running currently:
Copy
lsof -i :2121 | grep LISTEN
and you would see outputs similar to this if there is a node running currently:
Copy
nubit   1731890 root  410u  IPv6 69875760      0t0  TCP *:scientia-ssdb (LISTEN)
nubit   1731890 root  412u  IPv4 69875759      0t0  TCP *:scientia-ssdb (LISTEN)
If nothing shows, there is no node running now. You can skip the steps below.
Ok, there is a node currently running on your env and you want to stop it. Type in the following command to stop the node:
Copy
kill $(lsof -t -i:2121)
To check whether the node is stopped properly, type in:
Copy
lsof -i :2121 | grep LISTEN
If nothing shows up, the node is stopped, otherwise not.
Restarting a Background-running Node
Firstly, stop the running node according to Stopping a Background-running Node
Then, start the node again according to Starting a Node in Background
Process of Running nubit.sh (The First Time)
Here is the entire process of what the nubit.sh script does when first executed.
Start the Script:
The script begins with a message "Starting Nubit node..." to indicate that the process has started.
Determine the Architecture:
The script uses the uname command to determine the system's architecture and operating system and sets the ARCH_STRING variable based on the detected architecture:
darwin-arm64 for ARM-based macOS

darwin-x86_64 for Intel-based macOS

linux-arm64 for ARM-based Linux

linux-x86_64 for 64-bit Linux

linux-x86 for 32-bit Linux

If the ARCH_STRING is not set (indicating an unsupported architecture), the script prints an error message and exits.

Download and Extract Nubit Node Package:

The script checks if the nubit-node directory already exists:

If it does not exist, it constructs the URL for the appropriate Nubit node package based on the ARCH_STRING and prints the URL.

It then downloads the package using either curl or wget, whichever is available.

After downloading, it extracts the package using tar, renames the extracted directory to nubit-node, and deletes the tar file.

Make Necessary Installation:

Once the package is extracted, the following files and directories will be present in the nubit-node-linux-x86_64 directory:

bin/: Contains executable files for the node.

nubit: The main executable file for the Nubit node.

nkey: A utility for key management.

Update and Validate Nubit Node:

The script includes MD5 checksums for verifying the downloaded files.

If the checksums do not match, the script will download the latest version of the Nubit node to ensure optimal performance and access to new features.

The script constructs the URL for the appropriate Nubit node package based on the ARCH_STRING, downloads, and extracts it.

Download and Extract Light Node Data:

The script downloads the light node data from the URL https://nubit.sh/nubit-data/lightnode_data.tgz.

It then extracts the data, ensuring the node has the necessary data to start. For specific environment variables, please refer to set environment.

Start the Nubit Light Node:

The script navigates into the $HOME/nubit-node directory and runs the nubit executable to start the Nubit node with the appropriate parameters for a light node.

Initialize the Nubit Light Node:

The output includes several initialization messages:

The light node store is initialized.

A new Nubit address named my_nubit_key is generated, along with its mnemonic phrase (which is displayed once and must be saved securely).

The node joins the Nubit Alpha Testnet and starts syncing headers.

The script prints various status messages about the initialization process, including accessing the keyring, generating new keys, and starting the node.

Configure Node Settings:

The config.toml file, which is used by the Nubit node, includes various configurations such as startup timeout, shutdown timeout, P2P settings, RPC settings, and other node-specific configurations. This file ensures the Nubit node operates with the specified settings and parameters.

Example configurations in config.toml include:

[Node]: Startup and shutdown timeouts.

[Core]: IP, RPC port, and GRPC port.

[State]: Keyring account name and backend.

[P2P]: Listen to addresses, announce addresses, peer settings, etc.

[RPC]: Address and port.

[Share], [Header], [DASer], [Pruner]: Additional settings for data availability sampling, header synchronization, pruning, etc.

Output and Save PUBKEY and AUTHKEY:

The script outputs the generated PUBKEY and AUTHKEY, which are essential for node operations and security.

Ensure these keys are saved securely.

Node Information:

The script prints the following information about the node:

Copy
/_____/  /_____/  /_____/  /_____/  /_____/ 

Started nubit DA node 
node version: 
node type:      light
network:        nubit-alphatestnet-1

/_____/  /_____/  /_____/  /_____/  /_____/ 

The p2p host is listening on:
*  /ip4/127.0.0.1/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/tcp/2121/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/udp/2121/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/183.173.124.169/udp/58827/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/183.173.124.169/udp/58827/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/tcp/2121/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/udp/2121/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
Report Metrics and Print Head Information:

The script starts the Nubit node with metrics reporting enabled, sending metrics data to the endpoint otel.nubit-alphatestnet-1.com:4318.

The script prints information about the network headers and the syncing process, ensuring the node is up-to-date with the latest network state.

Process of Running nubit.sh (Subsequent Runs)
Notes:

Save Keys Securely: Always ensure that the PUBKEY and AUTHKEY are saved securely as they are crucial for node operations.

No Reinstallation Needed: Since the binaries are already verified, the script skips the reinstallation process, making subsequent runs faster.

Continued Operation: The node will continue from its previous state, ensuring seamless operation and synchronization with the network.

When you execute the nubit.sh script for the second time or any subsequent times, the process will differ slightly from the initial run. Here is a detailed guide for users executing the script after the first run:

Start the Script:

The script begins with a message "Starting Nubit node..." to indicate that the process has started.

Verify Installation:

The script checks the MD5 checksums of the existing nubit and nkey binaries to ensure they are correct.

If the checksums pass, the script prints "MD5 checking passed. Start directly" and proceeds without re-downloading the binaries.

Output Existing Keys:

The script confirms the presence of existing keys generated during the initial run.

Reminder: Ensure that the Nubit address, PUBKEY, and AUTHKEY are saved securely as they are crucial for node operations.
Initialize Node:

The script accesses the keyring to use the existing keys for node operations and constructs the keyring signer using the stored key information.

It initializes and opens the necessary databases using the Badger database, ensuring the node can access its stored data and continue from where it left off.

The script loads peers from disk, reconnecting the node with its known peers, joins the appropriate network topics, and starts the P2P client and server.

The node begins syncing headers from the network to stay up-to-date.

The script starts the DASer (Data Availability Sampler) from the last checkpoint and initializes the RPC server for communication.

Node Information:

The script also prints the node information as usual:

Copy
/_____/  /_____/  /_____/  /_____/  /_____/ 

Started nubit DA node 
node version: 
node type:      light
network:        nubit-alphatestnet-1

/_____/  /_____/  /_____/  /_____/  /_____/ 

The p2p host is listening on:
*  /ip4/127.0.0.1/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/tcp/2121/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/udp/2121/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/172.22.173.67/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/183.173.124.169/udp/58827/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip4/183.173.124.169/udp/58827/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/tcp/2121/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/udp/2121/quic-v1/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
*  /ip6/::1/udp/2121/quic-v1/webtransport/certhash/uEiB9lqoC_TeHP9bz4PWEDSUMlRMaOvVPhglAy63LtB-Qnw/certhash/uEiARTDqDNRTvmD7BKvwElMVEsySwmOY8AN6yvCPTs4Dp3w/p2p/12D3KooWSZmZWjyNbjy4t6mADjWzxWRa9KnfbwtTxtWhVh9Pc64i
Report Metrics and Print Head Information:

The script starts the Nubit node with metrics reporting enabled, sending metrics data to the endpoint otel.nubit-alphatestnet-1.com:4318.

The script prints information about the network headers and the syncing process, ensuring the node is up-to-date with the latest network state.
