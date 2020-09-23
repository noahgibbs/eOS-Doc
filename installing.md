## Installation

```
# Install eOS repository with recursive submodules

git clone git@github.com:ChatTheatre/eOS.git --recursive

# Move to eOS root folder

cd eOS

## Make sure all submodules are checked out to master branch

git submodule foreach --recursive git checkout master

## Move to the DGD source director

cd dgd/src

# make and install the current dgd which will be at …/eos/dgd/bin/dgd

make install

# return to repository root

cd ../..

# test dgd compiled, should get usage help message

./dgd/bin/dgd

# Add eos path to eos/eos.DGD

pwd
nano ./eos.dgd

# start dgd

./dgd/bin/dgd ./eos.dgd

# login telnet port

telnet local.host 50100

```
