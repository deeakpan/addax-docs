# Contract Source & ABIs

All Addax smart contracts are open source at [github.com/deeakpan/addax/tree/main/contracts](https://github.com/deeakpan/addax/tree/main/contracts).

## Source layout

```
contracts/
  v3-core/              Uniswap V3 factory and pool (0.7.6)
  src/
    addax/
      WzkLTC.sol        Custom wrapped native token
      AddaxAggregatorV3.sol  Multi-router aggregator
    aris/
      arisCore/
        reactors/       BaseReactor, LimitOrderReactor, DcaOrderReactor, DutchOrderReactor
        lib/            Order encoding, DCA structs, Dutch decay math
        base/           ReactorStructs, ProtocolFees, ReactorEvents
      permit2/          Permit2 signature and allowance transfer
      contracts/        Deployed wrappers (ArisPermit2, ArisLimitOrderReactor, etc.)
```

## Getting ABIs

The easiest way is to import from the compiled artifacts:

```typescript
import SwapRouterABI from "@uniswap/v3-periphery/artifacts/contracts/SwapRouter.sol/SwapRouter.json";
import QuoterV2ABI from "@uniswap/v3-periphery/artifacts/contracts/lens/QuoterV2.sol/QuoterV2.json";
import NPMABI from "@uniswap/v3-periphery/artifacts/contracts/NonfungiblePositionManager.sol/NonfungiblePositionManager.json";
```

For ARIS reactor contracts, the key ABI fragments are:

```typescript
const REACTOR_ABI = [
  "function execute((bytes order, bytes sig)) payable",
  "function executeWithCallback((bytes order, bytes sig), bytes callbackData) payable",
  "function executeBatch((bytes order, bytes sig)[]) payable",
  "function executeBatchWithCallback((bytes order, bytes sig)[], bytes callbackData) payable",
  "event Fill(bytes32 indexed orderHash, address indexed filler, address indexed swapper, uint256 nonce)",
];

const WZKLTC_ABI = [
  "function deposit() payable",
  "function withdraw(uint256 wad)",
  "function depositWrapped(uint256 wad)",
  "function withdrawWrapped(uint256 wad)",
  "function legacyReserve() view returns (uint256)",
  "function balanceOf(address) view returns (uint256)",
  "function approve(address, uint256) returns (bool)",
  "function transfer(address, uint256) returns (bool)",
  "function transferFrom(address, address, uint256) returns (bool)",
];
```

## Connecting (TypeScript)

```typescript
import { ethers } from "ethers";

const provider = new ethers.JsonRpcProvider("https://liteforge.rpc.caldera.xyz/http");
const signer = new ethers.Wallet(process.env.PRIVATE_KEY!, provider);

const swapRouter = new ethers.Contract(
  "0xD3B3b516d159AC5D0ddB579AA6feD92598202B55",
  SwapRouterABI.abi,
  signer
);

const npm = new ethers.Contract(
  "0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d",
  NPMABI.abi,
  signer
);
```

## Connecting (Python)

```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://liteforge.rpc.caldera.xyz/http"))

swap_router = w3.eth.contract(
    address=Web3.to_checksum_address("0xD3B3b516d159AC5D0ddB579AA6feD92598202B55"),
    abi=SWAP_ROUTER_ABI,
)
```
