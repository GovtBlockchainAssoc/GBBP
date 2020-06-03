# Installing GBAChain Node on Linux:

##<u>prerequisites:</u>

1) a recent Java installation (OpenJDK 13 or 14)

2) libsodium23 & libsodium-dev_1.0.16 (libraries)

for 1:

find your available versions of Java

    sudo apt search openjdk   # should show available versions of JDK/JRE (debian)
    sudo dnf search openjdk | grep latest | cut -f1 -d':'     # (Fedora/Redhat/CentOS)
    
you will likely need to download OpenJDK, I used the Zulu source:

https://www.azul.com/downloads/zulu-community/?&architecture=x86-64-bit&package=jdk

    sha256sum zulu13.29.9-ca-jdk13.0.2-linux_amd64.deb   # verify .deb checksum OK
    sudo dnf install zulu*linux.x86_64.rpm  # on Fedora, CentOS, RHEL
    sudo dpkg -i zulu*linux_amd64.deb  # on Debian 
    java --version 
    
should return something like this:
  
    openjdk 13.0.2 2020-01-14
    OpenJDK Runtime Environment Zulu13.29+9-CA (build 13.0.2+6-MTS)
    OpenJDK 64-Bit Server VM Zulu13.29+9-CA (build 13.0.2+6-MTS, mixed mode, sharing)

for 2:
    
on Linux Mint 17 'Qiana' (based on Ubuntu 14.04 "trusty"), I installed

    libsodium23_1.0.16-0ppa3_trusty1_amd64.deb 
    libsodium-dev_1.0.16-0ppa3_trusty1_amd64.deb

via 

    sudo dpkg -i libsodium23_1.0.16-0ppa3_trusty1_amd64.deb
    sudo dpkg -i libsodium-dev_1.0.16-0ppa3_trusty1_amd64.deb     
    
from 

https://launchpad.net/~phoerious/+archive/ubuntu/keepassxc/+sourcepub/8814980/+listing-archive-extra


##<u>Node Set Up</u>

using instructions from this page:

https://wiki.hyperledger.org/display/BESU/Building+from+source

BACKUP: your keys, genesis.json file, and static-nodes.json before re-installing!!!

    git clone --recursive https://github.com/hyperledger/besu GBAChain_Besu
    cd GBAChain_Besu
    ./gradlew build 
    ./gradlew integrationTest
    ./gradlew installDist   # destroys existing node files in build/install/besu folder!
    cd build\install\besu
    ./bin/besu --help

should return the besu node command set

if you need to recompile, use

    ./gradlew build --rerun-tasks  

  
  1. GBA Istanbul Byzantine Fault-Tolerant (IBFT) genesis block, save as GBA_IBFT_genesis.json
  
{
  "config": {
    "chainId": 2020,
    "constantinoplefixblock": 0,
    "ibft2": {
      "blockperiodseconds": 5,
      "epochlength": 30000,
      "requesttimeoutseconds": 10
    }
  },
  "nonce": "0x0",
  "timestamp": "0x58ee40ba",
  "extraData": "0xf83ea00000000000000000000000000000000000000000000000000000000000000000d5944f12efc5443be3c7e039508c080b2c9d95dd777f808400000000c0",
  "gasLimit": "0x1fffffffffffff",
  "difficulty": "0x1",
  "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "alloc": {}
}  
     

##<u>Node Connection to the PoA Network</u>

open port 30303 in your firewall:
    sudo ufw allow 30303

(in progress)

make a static-nodes.json file in your build/install/besu folder  
  
    [
    "enode://d5e7a620b32872393f2d2bf70d7ea0f29864802cf2230deea3ef4bc708abba99953baa21e051b32533142bd9176fee2acf4be4cd2506f32f8f333cc285fec502@40.79.33.82:30300",
    "enode://e377295d51c9f4a9f7429b5cd2b10f86f12f3cd4e51228e8d9aabb61a9e71698604aa131ac9f6017febfd0138bec7f6207c27b11503b58baa333de5347e6bac5@52.177.65.115:30300"
    ]


admin eth address: 0xc13b4e4cB7c2AF0D90A293d4d94De61Bf5317504

network info URL : http://etht5zt7j-dns-reg1.eastus2.cloudapp.azure.com:3001/networkinfo

standard options

    --identity=2           2 for EnLedger (?)
    --network-id=<BIG INTEGER>    # get from somewhere (?)
    --node-private-key-file=<PATH>
                           The node's private key file (default:
                           a file named key" in the Besu data folder)
          
    --genesis-file=<FILE>  Genesis file. requires --network-id to be set.
    --identity=<String>    Identification for this node in the Client ID    
    --key-value-storage=<keyValueStorageName>
                           Identity for the key-value storage to be used.
                        
    -l, --logging=INFO    [OFF, FATAL, ERROR, WARN,INFO, DEBUG, TRACE, ALL]
    --min-gas-price=<minTransactionGasPrice>
                           Minimum price (in Wei) offered by a transaction
                           for it to be included in a mined block (default: 1000)
                           
    --p2p-host=<HOST>      Ip address this node advertises to its peers
                           (default: 127.0.0.1)  (hmmmmmmmmm)

    --p2p-port=<PORT>      Port on which to listen for p2p communication
                           (default: 30303)
       
       
       
commands:

generates a keypair for the node, and a "key" file

    > besu public-key export   

generate extradata for genesis file, from validator addresses
    
    > besu rlp encode --from=./toEncode.json
    

URLs:

    http://etht5zt7j-dns-reg1.eastus2.cloudapp.azure.com:3001/networkinfo

Tutorial address:

    https://besu.hyperledger.org/en/stable/Tutorials/Private-Network/Create-IBFT-Network/
    https://besu.hyperledger.org/en/stable/HowTo/Configure/Consensus-Protocols/IBFT/

   
##<u>debug notes</u>

    task :besu:test

    org.hyperledger.besu.PrivacyReorgTest > reorgToShorterChain FAILED
    java.lang.UnsatisfiedLinkError at PrivacyReorgTest.java:140

    org.hyperledger.besu.PrivacyReorgTest > reorgToLongerChain FAILED
    java.lang.UnsatisfiedLinkError at PrivacyReorgTest.java:140

    org.hyperledger.besu.PrivacyReorgTest > reorgToChainAtEqualHeight FAILED
    java.lang.UnsatisfiedLinkError at PrivacyReorgTest.java:140

    org.hyperledger.besu.PrivacyReorgTest > privacyGroupHeadIsTracked FAILED
    java.lang.UnsatisfiedLinkError at PrivacyReorgTest.java:140


error report shows also:  
  
    java.lang.UnsatisfiedLinkError: libsodium.so: cannot open shared object file: No such file or directory

sudo apt-get install libsodium-dev

errort report now shows

    java.lang.LinkageError: Unsupported libsodium version 1.0.8 (9:1)

    apt search libsodium
    
SOLVED: 


installed

    libsodium23_1.0.16-0ppa3~trusty1_amd64.deb 
    libsodium-dev_1.0.16-0ppa3~trusty1_amd64.deb

from 

https://launchpad.net/~phoerious/+archive/ubuntu/keepassxc/+sourcepub/8814980/+listing-archive-extra

you may need to find a recent version of libsodium & libsodium-dev for your specific system

    
    
    
    
##<u>Setting up initial validator node:</u>
  
get install instructions for IBFT 2.0 node setup from online docs  
  
> besu rlp encode --from=./toEncode.json

to create initial "extra data" string from validator list
public eth addresses) in ["xx","xx","xx"] data file format

to get addresses in right format: 

> besu public-key export-address

example: 0x4f12efc5443be3c7e039508c080b2c9d95dd777f

remove 0x from the beginning of addresses from the toEncode.json validate list: 4f12efc5443be3c7e039508c080b2c9d95dd777f

ok started an IBFT node, maybe (?)

enode://fe639f8c0e7fc79ffd6eacbe7f49e82bba28a42b5a899c8f72694865b4d752d88ca050f866d36cf2856ad6bff3c3bb47ba46933fc30f81e3fcb715797985a929@63.227.14.139:30303


bootnode start command:  
  
> besu --genesis-file=/path/to/GBA_IBFT_genesis.json --p2p-host="server.external.ip.address"
  
peernode start command:  
  
put enode addresses in static-nodes.json in ["xx","xx","xx"] format

> besu --genesis-file=/path/to/GBA_IBFT_genesis.json




    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

