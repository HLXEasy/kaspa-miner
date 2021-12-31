# Kaspa-miner
[![Build status](https://github.com/elichai/kaspa-miner/workflows/ci/badge.svg)](https://github.com/elichai/kaspa-miner/actions)
[![Latest version](https://img.shields.io/crates/v/kaspa-miner.svg)](https://crates.io/crates/kaspa-miner)
![License](https://img.shields.io/crates/l/kaspa-miner.svg)
[![dependency status](https://deps.rs/repo/github/elichai/kaspa-miner/status.svg)](https://deps.rs/repo/github/elichai/kaspa-miner)

A Rust binary for file encryption to multiple participants. 


## Installation
### From Sources
With Rust's package manager cargo, you can install CUDA supported kaspa-miner via:

```sh
rustup default nightly
cargo install --git https://github.com/tmrlvi/kaspa-miner.git
```

This version includes a precompiled PTX, which would work with most modern GPUs. To compile the PTX youself, 
you have to clone the project:

```sh
git clone https://github.com/tmrlvi/kaspa-miner.git
cd kaspa-miner
# Using cuda nvcc that supports sm_30 (e.g., 9.2)
nvcc kaspa-cuda-native/src/kaspa-cuda.cu -std=c++11 -O3 --restrict --ptx --gpu-architecture=compute_30 --gpu-code=sm_30 -o ./resources/kaspa-cuda-sm30.ptx -Xptxas -O3 -Xcompiler -O3
# Using cuda nvcc from a recent cuda (e.g. 11.5)
nvcc kaspa-cuda-native/src/kaspa-cuda.cu -std=c++11 -O3 --restrict --ptx --gpu-architecture=compute_61 --gpu-code=sm_61 -o ./resources/kaspa-cuda-sm61.ptx -Xptxas -O3 -Xcompiler -O3 
cargo build --release
```

The regular version is still available at
```sh
cargo install kaspa-miner
```

### From Binaries
The [release page](https://github.com/tmrlvi/kaspa-miner/releases) includes precompiled binaries for Linux, and Windows (for the GPU version).


# Usage
To start mining you need to run [kaspad](https://github.com/kaspanet/kaspad) and have an address to send the rewards to.
There's a guide here on how to run a full node and how to generate addresses: https://github.com/kaspanet/docs/blob/main/Getting%20Started/Full%20Node%20Installation.md

Help:
```
kaspa-miner 0.2.1-GPU-0.1
A Kaspa high performance CPU miner

USAGE:
    kaspa-miner [FLAGS] [OPTIONS] --mining-address <mining-address>

FLAGS:
    -d, --debug                   Enable debug logging level
    -h, --help                    Prints help information
        --mine-when-not-synced    Mine even when kaspad says it is not synced, only useful when passing `--allow-submit-
                                  block-when-not-synced` to kaspad  [default: false]
        --no-gpu                  Disable GPU miner [default: false]
        --testnet                 Use testnet instead of mainnet [default: false]
    -V, --version                 Prints version information
        --workload-absolute       The values given by workload are not ratio, but absolute number of nonces [default:
                                  false]

OPTIONS:
        --cuda-device <cuda-device>...         Which CUDA GPUs to use [default: all]
        --devfund <devfund-address>            Mine a percentage of the blocks to the Kaspa devfund [default:
                                               kaspa:pzhh76qc82wzduvsrd9xh4zde9qhp0xc8rl7qu2mvl2e42uvdqt75zrcgpm00]
        --devfund-percent <devfund-percent>    The percentage of blocks to send to the devfund [default: 1]
    -s, --kaspad-address <kaspad-address>      The IP of the kaspad instance [default: 127.0.0.1]
    -a, --mining-address <mining-address>      The Kaspa address for the miner reward
    -t, --threads <num-threads>                Amount of miner threads to launch [default: number of logical cpus]
        --opencl-device <opencl-device>...     Which OpenCL GPUs to use (only GPUs currently. experimental) [default:
                                               none]
    -p, --port <port>                          Kaspad port [default: Mainnet = 16111, Testnet = 16211]
        --workload <workload>...               Ratio of nonces to GPU possible parrallel run [defualt: 16]
```

To start mining you just need to run the following:

`./kaspa-miner --mining-address kaspa:XXXXX`

This will run the miner on all the available CPU cores.

# Devfund

The devfund is a fund managed by the Kaspa community in order to fund Kaspa development <br>
A miner that wants to mine a percentage into the dev-fund can pass the following flags: <br>
`kaspa-miner --mining-address= XXX --devfund=kaspa:precqv0krj3r6uyyfa36ga7s0u9jct0v4wg8ctsfde2gkrsgwgw8jgxfzfc98` <br>
and can pass `--devfund-precent=XX.YY` to mine only XX.YY% of the blocks into the devfund (passing `--devfund` without specifying a percent will default to 1%)

# Donation Addresses

**Elichai**: `kaspa:qzvqtx5gkvl3tc54up6r8pk5mhuft9rtr0lvn624w9mtv4eqm9rvc9zfdmmpu`

**HauntedCook**: `kaspa:qz4jdyu04hv4hpyy00pl6trzw4gllnhnwy62xattejv2vaj5r0p5quvns058f`