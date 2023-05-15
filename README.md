# Deploy a Sovereign DEX Rollup on top of Blockrace Network

We choose to depoly an Ethermint rollup with a Uniswap V3 function on top of the Blockrace Network. 

# Installation & Setting Up

You can install Ethermint, Golang, Foundry, and learn how to run light client from this link: https://rollkit.dev/docs/tutorials/ethermint/. 

Here, i'd like to give some tips from my own experience. 

## Light client
Instead of following official docs, the recommoded way to launch a light client is as follows. 
```
celestia light start --gateway --gateway.addr 0.0.0.0 --gateway.port 26659 --metrics.tls=false --metrics --metrics.endpoint otel.celestia.tools:4319 --p2p.network blockspacerace

```
Then add rpc to config.toml to [Core] section
```
[Core]
    IP = "http://rpc-blockspacerace.mzonder.com"
    RPCPort = "30159"
    GRPCPort = "30190"
```

This would give you a stable connectioned light client: 
![image](https://github.com/maaaagi/celestiablockrace/blob/master/lightclient.png)


## Ethermint Rollup
The recommended way to lauch a Ethermint Rollup is as follows: 
```
ethermintd start --rollkit.aggregator true --rollkit.da_layer celestia --rollkit.da_config='{"base_url":"http://localhost:26659","timeout":60000000000,"gas_limit":6000000,"fee":6000}' --rollkit.namespace_id $NAMESPACE_ID --rollkit.da_start_height $DA_BLOCK_HEIGHT
```
Notes that you can 
