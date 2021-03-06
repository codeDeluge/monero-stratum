# monero-stratum

High performance CryptoNote mining stratum with Web-interface written in Golang.

**Stratum feature list:**

* Be your own pool
* Rigs availability monitoring
* Keep track of accepts, rejects, blocks stats
* Easy detection of sick rigs
* Daemon failover list
* Concurrent shares processing
* Beautiful Web-interface

![](https://cdn.pbrd.co/images/jRU3qJj83.png)

## Installation

Dependencies:

  * go >= 1.6
  * Everything required to build Monero
  * Monero >= **v0.11.0.0**

### Linux

Use Ubuntu 16.04 LTS.

Compile Monero source (with shared libraries option):

    apt-get install git cmake build-essential libssl-dev pkg-config libboost-all-dev
    git clone https://github.com/monero-project/monero.git
    cd monero
    git checkout tags/v0.11.0.0 -b v0.11.0.0
    cmake -DBUILD_SHARED_LIBS=1 .
    make

Install Golang and required packages:

    sudo apt-get install golang

Clone stratum:

    git clone https://github.com/sammy007/monero-stratum.git
    cd monero-stratum

Build stratum:

    MONERO_DIR=/path/to/monero cmake .
    make

`MONERO_DIR=/path/to/monero` is optional, not needed if both `monero` and `monero-stratum` is in the same directory like `/opt/src/`. By default make will search for monero libraries in `../monero`. You can just run `cmake .`.

### Mac OS X

Compile Monero source:

    git clone https://github.com/monero-project/monero.git
    cd monero
    git checkout tags/v0.11.0.0 -b v0.11.0.0
    cmake .
    make

Install Golang and required packages:

    brew update && brew install go

Clone stratum:

    git clone https://github.com/sammy007/monero-stratum.git
    cd monero-stratum

Build stratum:

    MONERO_DIR=/path/to/monero cmake .
    make

### Running Stratum

    ./build/bin/monero-stratum config.json

If you need to bind to privileged ports and don't want to run from `root`:

    sudo apt-get install libcap2-bin
    sudo setcap 'cap_net_bind_service=+ep' /path/to/monero-stratum

## Configuration

Configuration is self-describing, just copy *config.example.json* to *config.json* and run stratum with path to config file as 1st argument.

```javascript
{
  // Address for block rewards
  "address": "YOUR-ADDRESS-NOT-EXCHANGE",
  // Don't validate address
  "bypassAddressValidation": true,
  // Don't validate shares
  "bypassShareValidation": true,

  "threads": 2,

  "estimationWindow": "15m",
  "luckWindow": "24h",
  "largeLuckWindow": "72h",

  // Interval to poll daemon for new jobs
  "blockRefreshInterval": "1s",

  "stratum": {
    // Socket timeout
    "timeout": "15m",

    "listen": [
      {
        "host": "0.0.0.0",
        "port": 1111,
        "diff": 5000,
        "maxConn": 32768
      },
      {
        "host": "0.0.0.0",
        "port": 3333,
        "diff": 10000,
        "maxConn": 32768
      }
    ]
  },

  "frontend": {
    "enabled": true,
    "listen": "0.0.0.0:8082",
    "login": "admin",
    "password": "",
    "hideIP": false
  },

  "upstreamCheckInterval": "5s",

  "upstream": [
    {
      "name": "Main",
      "host": "127.0.0.1",
      "port": 18081,
      "timeout": "10s"
    }
  ]
}
```

### Donations

Donations are welcome.

**XMR**: `4Aag5kkRHmCFHM5aRUtfB2RF3c5NDmk5CVbGdg6fefszEhhFdXhnjiTCr81YxQ9bsi73CSHT3ZN3p82qyakHwZ2GHYqeaUr`

### License

Released under the GNU General Public License v2.

http://www.gnu.org/licenses/gpl-2.0.html
