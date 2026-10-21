# trinity-stack-devnet

The [Trinity stack descriptor](https://github.com/paritytech/trinity-user-agents/blob/main/docs/rfcs/trinity-stack-descriptor.md)
for PCF Devnet: the Paseo relay chain with Asset Hub (1000), People (1004) and Bulletin (1010), and the services the
Polkadot Community Foundation runs for its development builds.

```
https://stack.api.polkadotcommunity.foundation/.well-known/trinity-stack.json
```

The pointer, as a link or QR code:

```
polkadotapp://stack?key=0x1fbc5eed0deb09fe4eeefdf45350b4b4cf36370d9bbb93ca2818b076737a0ed3&url=https%3A%2F%2Fstack.api.polkadotcommunity.foundation%2F.well-known%2Ftrinity-stack.json
```

The descriptor is signed with the stack's key `0x1fbc5eed0deb09fe4eeefdf45350b4b4cf36370d9bbb93ca2818b076737a0ed3`, which readers holding the pointer check.

The RPC lists hold every public endpoint that reports the chain's genesis hash, has a finalized head within 50 blocks
of the best one seen, and serves both the `chainHead_v1` and the legacy `state_*`/`author_*` methods. People endpoints
also serve the statement store, because the descriptor marks that chain `statementStore`.

## Files

| File                             | What it is                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
| `model.json`                     | The source: the descriptor without `$v`, `sequence` and `generation` |
| `.well-known/trinity-stack.json` | The rendered descriptor this site serves                             |
| `publish-state.json`             | The last `sequence` and content hash, so `sequence` only rises       |

## Publishing a change

Edit `model.json`, sign it with the stack's key and commit all three files together. The generation is the relay chain's genesis hash.

```bash
truapi-host stack publish model.json \
  --generation 0x374057be67b355151f271ff70c3db98308c62c8adc48dc6724b6a009a1a014fd \
  --out . --state publish-state.json --signing-key-file <key file>
```

Check what is served:

```bash
truapi-host stack verify https://stack.api.polkadotcommunity.foundation/.well-known/trinity-stack.json --key 0x1fbc5eed0deb09fe4eeefdf45350b4b4cf36370d9bbb93ca2818b076737a0ed3
```
