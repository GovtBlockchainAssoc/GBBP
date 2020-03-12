# Installing GBAChain Node on Linux:

##<u>prerequisites:</u>

a recent Java installation (OpenJDK 13)
libsodium23 & libsodium-dev_1.0.16 (libraries)

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
    
on Linux Mint 17 'Qiana' (based on Ubuntu 14.04 "trusty"), I installed

    libsodium23_1.0.16-0ppa3~trusty1_amd64.deb 
    libsodium-dev_1.0.16-0ppa3~trusty1_amd64.deb

via 

    sudo dpkg -i libsodium23_1.0.16-0ppa3~trusty1_amd64.deb
    sudo dpkg -i libsodium-dev_1.0.16-0ppa3~trusty1_amd64.deb     
    
from 

https://launchpad.net/~phoerious/+archive/ubuntu/keepassxc/+sourcepub/8814980/+listing-archive-extra


##<u>Node Set Up</u>

using instructions from this page:

https://wiki.hyperledger.org/display/BESU/Building+from+source

    git clone --recursive https://github.com/hyperledger/besu GBAChain_Besu
    cd GBAChain_Besu
    ./gradlew build 
    ./gradlew integrationTest
    ./gradlew installDist
    cd build\install\besu
    ./bin/besu --help

should return the besu node command set

if you need to recompile, use

    ./gradlew build --rerun-tasks  
   



##<u>Node Connection to the PoA Network</u>


(in progress)



   
   
   
   
   
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

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

