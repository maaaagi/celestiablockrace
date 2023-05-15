# Deploy a Sovereign DEX Rollup on top of Blockrace Network

We choose to depoly an Ethermint rollup with a Uniswap V3 function on top of the Blockrace Network. 

# Installation 

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
www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F29874c7b-77fa-47cb-84b2-ab13c1bbdfb8%2FUntitled.png?id=9b1607b9-cbfa-4333-92ce-b6bbff283a5e&table=block&spaceId=8dedd799-f5dc-4814-ac8c-115b4dd53c6b&width=2000&userId=d8a846cf-f1b5-4b9e-a994-f90cac7b2422&cache=v2

