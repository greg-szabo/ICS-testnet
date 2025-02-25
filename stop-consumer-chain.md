This command will let you vote for a proposal to stop a consumer chain using the CLI

0. See the IBC client of the consumer chain

```
interchain-security-pd q ibc client states --home $PROV_NODE_DIR
```

1. See proposals on the blockchain
```
PROV_NODE_DIR=~/provider

interchain-security-pd q gov proposals --home $PROV_NODE_DIR
```
You should see a proposal called "Stop the first consumer chain!". Check what the `proposal_id` is, and input it as `PROP_ID` below.

2. Now vote for the proposal

```
PROP_ID=3
PROV_KEY=provider-key
PROV_CHAIN_ID=provider

interchain-security-pd tx gov vote $PROP_NUMBER yes --from $PROV_KEY \
       --keyring-backend test --chain-id $PROV_CHAIN_ID --home $PROV_NODE_DIR -b block -y
```

3. Check that the IBC client of the consumer chain has been stopped

```
interchain-security-pd q ibc client states --home $PROV_NODE_DIR
```

After the consumer chain is stopped, the `latest_height.revision_height` field will stop updating, and will be the same every time you query.


4. Check that the consumer chain has stopped
```
cat ${CONS_NODE_DIR}/logs | grep -m 1 "shutdown consumer chain since it is not secured anymore"
```

You will see where the consumer chain stopped itself once the channel was closed. For example: `4:46PM ERR CCV channel "channel-0" was closed - shutdown consumer chain since it is not secured anymore`
