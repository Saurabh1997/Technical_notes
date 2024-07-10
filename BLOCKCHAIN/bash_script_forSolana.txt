#!/bin/bash

# Set nameservers in /etc/resolv.conf
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
echo "nameserver 8.8.4.4" | sudo tee -a /etc/resolv.conf

# Update package lists and install required packages
sudo apt-get update
sudo apt-get install -y \
    build-essential \
    pkg-config \
    libudev-dev \
    llvm \
    libclang-dev \
    protobuf-compiler \
    libssl-dev

# Install Rust using curl
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# Add Rust binaries to the PATH (source the profile script)
source $HOME/.cargo/env

# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"

# Install Anchor AVM
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force

# Install the latest version of Anchor AVM
avm install latest

# Set the installed version of Anchor AVM as the active version
avm use latest

# Display Rust, Solana, and Anchor versions
rustc --version
cargo --version
solana --version
avm --version
