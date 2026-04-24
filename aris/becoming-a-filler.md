# Becoming a Filler

A filler is an offchain bot that watches the Addax order API, decodes open orders, and calls `execute()` on the appropriate reactor contract when it can profitably fill them.

## What you need

- A funded wallet on LitVM LiteForge (chain ID 4441) with enough zkLTC for gas.
- Sufficient token inventory to deliver order outputs (or a flash loan / DEX integration to source them).
- An RPC connection: `https://liteforge.rpc.caldera.xyz/http`

## Libraries

```bash
npm install ethers axios
```

For Python:
```bash
pip install web3 requests
```

## Minimal filler loop (TypeScript)

```typescript
import { ethers } from "ethers";
import axios from "axios";

const RPC = "https://liteforge.rpc.caldera.xyz/http";
const CHAIN_ID = 4441;
const API = "https://addax.finance/api";

const provider = new ethers.JsonRpcProvider(RPC);
const wallet = new ethers.Wallet(process.env.PRIVATE_KEY!, provider);

const REACTOR_ABI = [
  "function execute((bytes order, bytes sig) order) payable",
  "function executeBatch((bytes order, bytes sig)[] orders) payable",
];

const reactors: Record<string, string> = {
  Limit:    "0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502",
  Dutch_V2: "0xA70c28ef82d1427Fa68F65960FbfFb1198349593",
  DCA:      "0x99D007fD09211B437Ecc415bB8D08Cc68b877672",
};

async function pollAndFill() {
  const { data } = await axios.get(`${API}/orders`, {
    params: { chainId: CHAIN_ID, orderStatus: "open", limit: 100 },
  });

  for (const order of data.orders) {
    const reactorAddress = reactors[order.orderType];
    if (!reactorAddress) continue;

    // Decode and check if profitable — implement your own logic here
    const isProfitable = await checkProfitability(order);
    if (!isProfitable) continue;

    const reactor = new ethers.Contract(reactorAddress, REACTOR_ABI, wallet);

    try {
      const tx = await reactor.execute({
        order: order.encodedOrder,
        sig: order.signature,
      });
      await tx.wait();
      console.log(`Filled ${order.orderHash} in ${tx.hash}`);
    } catch (err) {
      console.error(`Failed to fill ${order.orderHash}:`, err.message);
    }
  }
}

// Poll every 5 seconds
setInterval(pollAndFill, 5000);
```

## Minimal filler loop (Python)

```python
import time
import requests
from web3 import Web3

RPC = "https://liteforge.rpc.caldera.xyz/http"
API = "https://addax.finance/api"
PRIVATE_KEY = "0x..."

w3 = Web3(Web3.HTTPProvider(RPC))
account = w3.eth.account.from_key(PRIVATE_KEY)

REACTOR_ABI = [{"name":"execute","type":"function","inputs":[{"name":"order","type":"tuple","components":[{"name":"order","type":"bytes"},{"name":"sig","type":"bytes"}]}],"outputs":[]}]

REACTORS = {
    "Limit":    "0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502",
    "Dutch_V2": "0xA70c28ef82d1427Fa68F65960FbfFb1198349593",
    "DCA":      "0x99D007fD09211B437Ecc415bB8D08Cc68b877672",
}

def poll_and_fill():
    res = requests.get(f"{API}/orders", params={"chainId": 4441, "orderStatus": "open", "limit": 100})
    orders = res.json().get("orders", [])

    for order in orders:
        reactor_address = REACTORS.get(order["orderType"])
        if not reactor_address:
            continue

        reactor = w3.eth.contract(address=Web3.to_checksum_address(reactor_address), abi=REACTOR_ABI)

        try:
            tx = reactor.functions.execute((
                bytes.fromhex(order["encodedOrder"][2:]),
                bytes.fromhex(order["signature"][2:]),
            )).build_transaction({
                "from": account.address,
                "nonce": w3.eth.get_transaction_count(account.address),
                "gas": 500_000,
            })
            signed = account.sign_transaction(tx)
            tx_hash = w3.eth.send_raw_transaction(signed.raw_transaction)
            print(f"Filled {order['orderHash']} -> {tx_hash.hex()}")
        except Exception as e:
            print(f"Failed {order['orderHash']}: {e}")

while True:
    poll_and_fill()
    time.sleep(5)
```

## Using executeWithCallback

If you need to source output tokens just-in-time (e.g. from a DEX swap), implement the `IReactorCallback` interface:

```solidity
interface IReactorCallback {
    function reactorCallback(ResolvedOrder[] calldata orders, bytes calldata callbackData) external;
}

contract MyFiller is IReactorCallback {
    function reactorCallback(ResolvedOrder[] calldata orders, bytes calldata callbackData) external {
        // At this point, input tokens have been transferred to this contract.
        // Swap them on Addax pools and return the output tokens to the reactor.
        for (uint i = 0; i < orders.length; i++) {
            // ... your swap / sourcing logic
        }
    }
}
```

Then call `executeWithCallback(signedOrder, callbackData)` from your filler contract.

## Batch filling

Fill multiple orders in one transaction to save gas:

```typescript
const tx = await reactor.executeBatch(
  orders.map(o => ({ order: o.encodedOrder, sig: o.signature }))
);
```

## Profitability check

At minimum, verify:
1. `order.deadline` has not passed.
2. You can source the output tokens at a cost below `inputAmount × (1 - margin)`.
3. Gas cost does not exceed the spread.

Use `AddaxV3QuoterV2` (`0x3Fb614d43146792FC0EfAe84f076e24085FD73A2`) to simulate swap output before committing.
