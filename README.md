# Deploy a Sovereign DEX Rollup on top of Blockspace Race Network

We have chosen to deploy an Ethermint rollup with a Uniswap V3 function on top of the Blockspace Race Network.

## Installation & Setting Up

You can install Ethermint, Golang, and Foundry, and learn how to run a light client from this link: https://rollkit.dev/docs/tutorials/ethermint/. 

Here, I would like to share some tips from my own experience.

### Light client
Instead of following official documentation, the recommended way to launch a light client is as follows.

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

This will provide you with a stable and connected light client: 

![image](https://github.com/maaaagi/celestiablockrace/blob/master/lightclient.png)


### Ethermint Rollup
The recommended way to launch an Ethermint Rollup is as follows: 
```
ethermintd start --rollkit.aggregator true --rollkit.da_layer celestia --rollkit.da_config='{"base_url":"http://localhost:26659","timeout":60000000000,"gas_limit":6000000,"fee":6000}' --rollkit.namespace_id $NAMESPACE_ID --rollkit.da_start_height $DA_BLOCK_HEIGHT
```
Notes: If you encounter these errors, you can switch to another RPC, such as localhost:26658: 

![image](https://github.com/maaaagi/celestiablockrace/blob/master/error.png) 

## Deploy smart contracts with UniswapV3 functions on the Ethermint sovereign rollup with Foundry

You can run this script ( ensure Blockrace Network is connecting in another terminal window):

```
$ forge script scripts/DeployDevelopment.s.sol --broadcast --fork-url <YOU_RURL> --private-key <YOUR_PRIVATE_KEY>
```

Here we successfully deployed our contracts into Blockspace Race Network. 

![image](https://github.com/maaaagi/celestiablockrace/blob/master/depolying.png)

### Interact with the smart contract
Once you have deployed the smart contracts in this repository, you can interact with them by ``` cast ```

For example, you can check the current price and tick of a pool: 
```
$ cast call POOL_ADDRESS "slot0()"| xargs cast --abi-decode "a()(uint160,int24)"
5602277097478614198912276234240
85176
```
Or you can also use the ```eth_call``` JSON-RPC method to make the call if you want to check the USDC balance of a targeted address. 
```
$ params='{"from":"0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266","to":"0xe7f1725e7734ce288f8367e1bb143e90bb3f0512","data":"0x70a08231000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb92266"}'
$ curl -X POST -H 'Content-Type: application/json' \
  --data '{"id":1,"jsonrpc":"2.0","method":"eth_call","params":['"$params"',"latest"]}' \
  http://127.0.0.1:8545
{"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000011153ce5e56cf880000"}
```

## Ends
Now you can play around with this repository and start building your own rollups with specified contracts. 





