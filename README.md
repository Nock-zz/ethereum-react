# ethereum+react



INSTALLATION OF VARIOUS TOOLS

1) Install go-ethereum

https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu

sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum

type geth version to check it is installed

2) Install truffle's ganache

https://truffleframework.com/ganache

click on download to download an app image.

make the app image an executable (right-click and tick box)

3) Install Node.js
# Ubuntu
curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -

sudo apt-get install -y nodejs

# use sudo on Ubuntu or run this as root on debian
sudo apt-get install -y build-essential

check the install with:
node -v
npm -v


4) Install truffle 4

sudo npm -g uninstall truffle
sudo npm -g install truffle

check the install with:
truffle version

5) Install atom

https://github.com/atom/atom

sudo add-apt-repository ppa:webupd8team/atom
sudo apt update
sudo apt install atom

check with:
which atom
which apm #atom package manager

run:
apm install language-ethereum #to get solidity support

6) Set up a private geth nodejs

mkdir -p private

cd private

7) Create the genesis block with puppeth (tool laoded with geth)

First check you are in the private directory.

puppeth

Network Name?
DevChain

What would you like to do?
2  #Configure new genesis

Which consensus engine?
1  #Ethash

Which accounts should be pre-funded?
0x #Type Enter

Specify your chain/network id
4224 # Anything but 1,2,3,4,42

Anything fun to embed in genesis block?
    # Hit Enter

What would you like to do?
2 #manage existing genesis

2 # export genesis configuration

Which file to save to? default DevChain.json
   # Hit Enter

Quit with Ctrl+C

List the directory

ls -al


6) Create the private node with geth

geth -- datadir . init DevChain.json

7) Create 3 new accounts

one to hold mining rewards, one to act as a buyer and one to act as a seller

geth --datadir . account new
Use a common password (pass1234) for all three accounts

These three accounts will be shown in the keystore dir.

To list the accounts:

geth datadir . account list

8) Create startnode.sh

Create startnode.sh (type atom startnode.sh)

geth --networkid 4224 --mine --minerthreads 2 --datadir "." --nodiscover --rpc --rpcport "8545" --port "30303" --rpccorsdomain "*" --nat "any" --rpcapi eth,web3,personal,net --unlock 0 --password ./password.sec --ipcpath "~/.ethereum/geth.ipc"

"*
We use "geth". It has a first argument that's called "--networkid",

and here we'll use "4224". We want our node to mine right away so

we'll use the "--mine" option. And we can also specify how many threads it will

use on our machine, so here I will specify "--minerthreads 2" for example to

use 2 threads. You decide the number. The default is 3 I think. Then we need to

specify the "--datadir" again, and since we will run this from the "private" directory

every time, we can just specify ".", because it is saved inside the "private"

directory. Then we want this node to be isolated, not to connect with any peer, so

we'll deactivate the discovery protocol, so "--nodiscover". But we want to enable

the RPC endpoint, because we'll use it to connect to it from the MetaMask

extension later: "--rpc". We also specify the RPC port which, we'll leave with the

default, so "--rpcport", and that will be "8545" within double-quotes. And even though the

discovery protocol is deactivated, we can also specify the discovery port, that is

by default "30303". Next thing, we want to be able to connect to the RPC

endpoint from any domain, so we need to add an option for that "--rpccorsdomain",

and that would be a "*".

"*
Then, there's this obscure "--nat" option that we'll

configure with "any". And then we need to enable a certain number of modules over

the RPC endpoints, and to do that we need to add "--rpcapi" as an option. And then we

can list the modules separated by a comma.

So "eth,web3,personal,net". So those are the 4

modules that will be enabled over the RPC endpoint. Then, since our node will

mine, it will need to unlock an account for adding blocks to the chain, and so we

can add the "--unlock" argument and specify the index of the account to unlock. And

it will also need a password so "--password", that will point to a file that

contains the password. And we'll create that file in a second. Actually, I think

we should remove the double quotes. And the last parameter is probably one of

the most important, it's the "--ipcpath". So when our private node will start, it

opens an IPC endpoint. So in addition to the RPC endpoint, it also opens an IPC

endpoint, that will be very useful for other Geth tools like Mist, or the Geth

console that we'll open soon. And basically what this does is specify the

path on which to create this IPC endpoint, so that other tools can find it

automatically. So here we need to specify a value that corresponds to the default

path where other tools will look for it. In our case, that will be in the user

home directory, under ".ethereum/geth.ipc". So by default all the other

Geth tools will look for this IPC endpoint under the ".ethereum"

directory of the user home directory, in the "geth.ipc" file. This is very

important. If you mess up that one, you will have problems later on when you

want to attach a console to your running node.

One other important point is to make sure that all this command fits on one

line, because if you put it on several lines, if you add some line feeds or carriage

returns to this file, everything that's not on the first line will be ignored.

9) Create password.sec in private

Create password.sec in atom.
Enter the password on the first line and save:
pass1234

10) make startnode.sh executable

chmod + x startnode.sh
















```
