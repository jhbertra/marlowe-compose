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
all services in the background. Use `docker-compose port proxy 3700` to find
out which local port the runtime binary protocol is being served on. You can then
pass this port to the `marlowe` command, or to `connectToMarloweRuntime` in the
Haskell client library.
