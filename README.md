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

Ubuntu

curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -

sudo apt-get install -y nodejs

Use sudo on Ubuntu or run this as root on debian

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
leave blank    # Hit Enter

What would you like to do?
2 #manage existing genesis

2 # export genesis configuration

Which file to save to? default DevChain.json
leave blank to accept default   # Hit Enter

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

11) run startnode with  

./startnode.sh


12) Start the console  

geth attach  

eth.accounts // lists accounts

eth.coinbase // lists accounts[0]

web3.fromWei(eth.getBalance(eth.coinbase), "ether");

web3.fromWei(eth.getBalance(eth.accounts[1]), "ether");

web3.fromWei(eth.getBalance(eth.accounts[2]), "ether");

miner.stop()

miner.start(2) // 2 threads

net.version // gets the network id 4224

personal.unlockAccount(eth.accounts[1], "pass1234", 300);  // unlocked for 300 seconds

personal.unlockAccount(eth.accounts[2]) // you must entr a password at the prompt

eth.sendTransaction({from:eth.coinbase, to:eth.accounts[1], value: web3.toWei(10, "ether")})

eth.sendTransaction({from:eth.coinbase, to:eth.accounts[2], value: web3.toWei(10, "ether")})

For additional documentation - see https://web3js.readthedocs.io/en/1.0/getting-started.html#adding-web3-js

13) Install Metamask (https://metamask.io)  

Create the metamask account with a random seed phrase and do not use the ganache default seed phrase.

Extract the private keys of ganache and use these to create 10 new accounts on metamask.
Alternatively get the seedphrase from metamask and use it in ganache.

You can also import the accounts created in geth from the keystore using account import in metamask.


14) Create a simple smart contract and deploy via node console

First create the directory of the project:  

mkdir greetings  

cd greetings

Set up npm:  

npm init  

npm install web3 solc@0.4.18

Create a .sol file for the code:  

atom Greetings.sol


Enter the contract code:
pragma solidity ^0.4.18;

contract Greetings {
  string message;

  function Greetings() public {
    message = "I'm ready!";
  }

  function setGreetings(string _message) public {
    message = _message;
  }

  function getGreetings() public view returns (string) {
    return message;
  }

}

Compile the contract with terminal:

make sure you are in the root directory of the Greetings project.  

Start a node console:  

$> node

Require web3:  

> Web3 = require('web3')  

> web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:7545")) // This is the port of the ganache in memory instance

List the accounts:  

> web3.eth.accounts

Require solc:  

solc = require('solc');

Read in the contract Greetings.sol:  

sourceCode = fs.readFileSync('Greetings.sol').toString()

Compile the Source Code:  

compiledCode = solc.compile(sourceCode)

Now pull out the contract ABI:  

contractABI = JSON.parse(compiledCode.contracts[':Greetings'].interface)

Now use web3.eth.contract to create a contract factory from the ABI above:  

greetingsContract = web3.eth.contract(contractABI)

Next extract the bytecode of the contract:  

byteCode = compiledCode.contracts[:Greetings].bytecode

Now Deploy the contract using the contract factory:  

greetingsDeployed = greetingsContract.new({data: byteCode, from: web3.eth.accounts[0], gas: 4700000})

You can check the contract in ganache but you can also check its address with:

greetingsDeployed.address

We now need to get an instance of the Greetings contract:

greetingsInstance = greetingsContract.at(greetingsDeployed.address)

With the instance we can get the message of the contract:  

greetingsInstance = getGreetings()

Now change the message:  

greetingsInstance.setGreetings('Hello world!', {from: web3.eth.accounts[0]})

The message should now be changed so check with:  

greetingsInstance = getGreetings()

Exit the console:  

.exit

15) Deploy the Greetings contract using truffle

First create a new project directory GreetingsTruffle:  

mkdir GreetingsTruffle

cd GreetingsTruffle

Set up truffle:    

truffle init  

Check the project:  

atom .

The GreetingsTruffle project has the following structure.

contracts directory

migrations directory

test directory

truffle.js //config file  

In the contracts directory add in the Greetings.sol file.  

In the migrations directory add in a new file:  

2_deploy_contracts.js

This has the following code:


//This follows the form of the 1_initial_migration.js.  
//N.B the path for Greetings.sol is relative to the source directory i.e. contracts  
var Greetings = artifacts.require('./Greetings.sol');


//N.B the deployer function is provider by truffle.  
module.exports = function(deployer) {
  deployer.deploy(Greetings);
};

Now we can deploy the contract to the development instance (ganache):  

$> truffle develop  

This starts an internal ethereum node on port 9545.
It has 10 accounts just like ganache.
It opens the truffle console.

Now we open a second truffle console on the GreetingsTruffle project root:  

$> truffle develop --log   

This also connects to the truffle development node on port 9545.  

We switch back to the original truffle console:  

> migrate --compile-all --reset  
// the last two flags force a recompile and a redeployment  

If we switch back to the console log we can see that there are four transactions. The second and fourth are created by truffle to track the migrations and GreetingsTruffle contract deployments by updating the last_completed_migration field of the Migrations contract.

In the truffle console the contract is identifed by the same name we used in the migration script (2_deploy_contracts). So as we defined var Greetings, we can use Greetings.

We can get the address:  

> Greetings.address

By the way, we can see the compiled file in the build directory, under the file name of Greetings.json.

At the very bottom you can see the network id of 4447 which is the network id of the internal truffle ethereum in memory node.

We can also use web3 methods:

> web3.eth.accounts  

and we get all the accounts

To get an instance of the contract we have to enter the following command:

Greetings.deployed().then(function(instance) {app = instance;});

Now we can call the getGreetings() function:  

app.getGreetings();

And also the setGreetings() function:

app.setGreetings("Hello you all!", {from: web3.eth.accounts[0]})

And we can recheck the message:

app.getGreetings();

We can exit the development console with:

> .exit  

We can also deploy to ganache.  
First start ganache.  

So we open truffle.js and we add the folliwng information:  
module_exports = {
networks: {
  ganache: {
    host: "localhost",
    port:7545,
    network_id: "*"
    }
  }
}

Now we can compile from the command prompt:  

First we type the following command:  

$>truffle migrate --compile-all --reset --network ganache

After deployemnt we have to run the truffle console on the ganache network and to do this we use:

$> truffle console --network ganache  

We can now call all the functions as before:

To get an instance of the contract we have to enter the same command that we used for the Truffle develop instance:  

Greetings.deployed().then(function(instance) {app = instance;});


app.getGreetings();

And also the setGreetings() function:

app.setGreetings("Hello you all!", {from: web3.eth.accounts[0]})

And we can recheck the message:

app.getGreetings();


































```
