# Example Docker Compose configuration for Marlowe Runtime

This repository contains a bare-bones docker compose manifest for running the
Marlowe Runtime. It also contains node configurations for `preview` and
`preprod` networks.


## How to use

First, set the `NETWORK` environment variable in your shell to either `preview`
or `preprod`. Please note that due to the volume of test Marlowe transactions
on `preview`, some components currently don't work on `preview`, so for a
stable experience, `preprod` is recommended currently.

Once the `NETWORK` variable is set, you can use `docker-compose up -d` to start
all services in the background. Ports are exposed for the `history`,
`discovery`, and `tx` services and can be connected to via the `marlowe` CLI or
the Haskell client library. Use e.g. `docker-compose port history 3717` to find
out which local port is bound to port `3717` of the `history` service.

## Note on scaling

One of the main things we want to investigate is the horizontal scalability of
the runtime services. Please note that the following should be adhered to when
scaling:

- We recommend running one instance of `chain-indexer` connected to one cardano node.
- Each `chain-indexer` instance must have its own postgresql database (they can
  be located on the same postgresql instance, but must be different databases).
- Multiple `chainseekd` instances can be run against one database. We recommend
  using a high-availability replicated postgres cluster for scaling to multiple
  `chainseekd` instances, with `chain-indexer` connected to the master node.
- Multiple instances of `history`, `discovery`, and `tx` can be run per `chainseekd` instance.
- Load balancing is out-of scope of the application code. Running multiple
  services requires putting a TCP load balancer in front of the ports. All that
  matters is that individual TCP connections should stay connected to the same
  destination node that it initially connects to, as the protocols are
  stateful.
