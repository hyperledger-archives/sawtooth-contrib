# Configure Grafana, Telegraf To The Sawtooth-Raft Network

The document helps you setup the Grafana to the sawtooth-raft network.
The setup is demonstrated using the instructions
[here](https://sawtooth.hyperledger.org/docs/raft/nightly/master/configuring_deploying.html#starting-a-multi-node-raft-network).

## Setup

### Step 0: Clone the repositories

Clone the [sawtooth-core](https://github.com/hyperledger/sawtooth-core),
[sawtooth-contrib](https://github.com/hyperledger/sawtooth-contrib)
and [sawtooth-raft](https://github.com/hyperledger/sawtooth-raft)
repositories to your work environment.

### Step 1: Bring up the admin service

```shell_script
$ cd sawtooth-raft/adhoc
$ docker-compose -f admin.yaml up -d
```

### Step 2: Bring up the Grafana and the InfluxDB services

```shell_script
$ cd sawtooth-contrib/raft/stats
$ SAWTOOTH_CORE=/path/to/the/repository/sawtooth-core docker-compose -f grafana.yaml up -d
```

### Step 3: Bring up the nodes as stated in sawtooth-raft documentation

**Note:** *Use the file `node-with-stats.yaml` instead of `node.yaml`
to get telegraf stats.*

Here are the instructions to use the `node-with-stats.yaml` file.
Bring up `n` number of non-genesis nodes, so that the logical volume
which is created in the `Step 1` has the required credentials for the
`genesis` node.

a. For non-genesis node

```shell_script
$ SAWTOOTH_RAFT=/path/to/the/repository/sawtooth-raft SAWTOOTH_CORE=/path/to/the/repository/sawtooth-core docker-compose -f node-with-stats.yaml -p <NODENAME> up
```

b. For genesis node

```shell_script
$ SAWTOOTH_RAFT=/path/to/the/repository/sawtooth-raft SAWTOOTH_CORE=/path/to/repository/sawtooth-core GENESIS=1 docker-compose -f node-with-stats.yaml -p <NODENAME> up
```

**Caution:**
Refer to the `sawtooth-raft` documentation for the explanation.
A `genesis` block is required for all the nodes to bootstrap the network,
notice how the nodes wait on the `genesis` block creation in
`node-with-stats.yaml` file.

### Step 4: Access Grafana

Go to the [http://localhost:3000](http://localhost:3000) link in your
favorite browser.
