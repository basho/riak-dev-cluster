Riak Dev Cluster for OS X
=========================

*NOTE: This project currently installs Riak 2.1.3*

Easily run a 5-node [Riak](http://www.basho.com/riak) cluster on OS X.

This repository uses the pre-packaged binaries for OS X to embed the
Erlang runtime. This eliminates the need to build Riak from source and
install Erlang. For convenience, the binaries can be found in the Basho
[Downloads](http://docs.basho.com/riak/latest/downloads/).

By default, the rake task installs 5 nodes of Riak running on a local
machine in a fashion similar to the `devrel` build target. In essence,
it creates five full Riak stacks (including Erlang virtual machines) to
run on one server to simulate a cluster. This is useful in a development
environment, but running Riak on a single server for benchmarking or
production use is counterproductive. This is discussed in greater detail
in a blog post entitled [*Riak Development Anti-Patterns*](http://basho.com/riak-development-anti-patterns/)

## Important Considerations
* The names of the nodes are riak[1-5]@127.0.0.1
* The HTTP port of riak1 is 11098: <http://127.0.0.1:11098>
* Ports on other nodes are 12098, 13098, 14098 & 15098.
* Protobuf port of riak1 is 11087.
* Handoff port of riak1 is 11099.
* https port is off, or same as http port.
* Riak Control (Admin UI) is available [here](http://127.0.0.1:11098/admin)
* All nodes use the [LevelDB storage backend](http://docs.basho.com/riak/latest/ops/advanced/backends/leveldb/)
* [Riak Search](http://docs.basho.com/riak/latest/dev/using/search/) is disabled 
* The `/etc/riak.conf` in each node's directory enumerates all other
  ports and settings

## Getting started

1) Ensure that Open File Limits are set per most recent documentation
recommendations. recommendations. This step, which is manual, is
important to ensure that Riak behaves properly. [Open File Limits](http://docs.basho.com/riak/latest/ops/tuning/open-files-limit/#Mac-OS-X)

2) Clone the repository and navigate to the folder:

```
$ git clone git://github.com/basho/riak-dev-cluster.git
$ cd riak-dev-cluster
```

3) There is a `bootstrap` command available that downloads the
pre-packaged OS X binary, installs, starts, and joins the Riak cluster
in one go:

```
rake bootstrap
```

4) Verify that all nodes have started, and joined the cluster:

````
rake member_status
````

5) Riak is up and running! You can [Test the Cluster](http://docs.basho.com/riak/latest/quickstart/#Test-the-Cluster)
or progress directly to [A Taste of Riak](http://docs.basho.com/riak/latest/dev/taste-of-riak/)

## Control commands

This project includes multiple rake tasks that make interacting with
your Riak cluster nearly as simple as operating a cluster in production.
Several of the common tasks are enumerated in greater detail below.

```
rake start
```

Start all nodes in the cluster using the Riak command line option [start](http://docs.basho.com/riak/latest/ops/running/tools/riak/#start):
`./riak[1-5]/bin/riak start`. 

```
rake stop
```

Stop all nodes in the cluster using the Riak command line option [stop](http://docs.basho.com/riak/latest/ops/running/tools/riak/#stop):
`./riak[1-5]/bin/riak stop`. 

```
rake clear
```

Clear all data by first completing the `rake stop` task and then
deleting directories (via rm -rf).

```
rake restart
```

Restart all nodes in the cluster without clearing all data by running
the `rake stop` task followed by the `rake start` task.

```
rake join
```

Join all nodes as a cluster using the Riak command line option
[join](http://docs.basho.com/riak/latest/ops/running/tools/riak-admin/#join)
with the force option. If the cluster is stopped or restarted, this
command only needs issued once. If the cluster is cleared, it must be
reissued:  `./riak[2-5]/bin/riak-admin join -f riak1@127.0.0.1`. 

## Riak Data Types

There are a few `rake` commands that will set up your cluster to use
[Riak Data Types](http://docs.basho.com/riak/2.0.0/dev/using/data-types/). You can
create a [bucket type](http://docs.basho.com/riak/2.0.2/dev/advanced/bucket-types/) for
using [Riak counters](http://docs.basho.com/riak/2.0.2/dev/using/data-types/#Counters)
called `counters`:

```
rake counter_bucket
```

You can do the same for sets:

```
rake set_bucket
```

And for maps:

```
rake map_bucket
```

## Other commands

See all available commands by running `rake -T` or just `rake` by itself.

### Note

Depending on your Erlang cookie, you may have to use the commands with `sudo`.

## Development

Basho Labs repos survive because of community contribution. This module
has extensive test coverage in order to make contributing easier. Please
read [CONTRIBUTING](CONTRIBUTING.md) for details. 

### Maintainers

* [Tyler Hannan](https://github.com/tylerhannan)

You can [read the full
guidelines](http://docs.basho.com/riak/latest/community/bugs/) for bug
reporting and code contributions on the Basho Docs. And **thank you!**
Your contribution is incredibly important to us.

## License and Authors

* [Erick Dennis](https://github.com/edennis)
* [Sebastian Röbke](https://github.com/boosty)
* [Simon Vans-Colina](https://github.com/simonvc)
* [Seth Thomas](https://github.com/cheeseplus)
* [Joel Jacobson](https://github.com/joeljacobson)
* [Tyler Hannan](https://github.com/tylerhannan)

Copyright (c) 2015 Basho Technologies, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied.

See the License for the specific language governing permissions and
limitations under the License.





