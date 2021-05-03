# Predeploy-contracts

Generate bytecode for predeployment of ERC20 smart contracts in Acala.

## Build

Run `yarn` to install dependencies.

## Generate bytecode

To generate bytecode, run `yarn run generate-bytecode`.

The generated bytecode JSON file would be `./resources/bytecodes.json`.

## Development

The token list for ERC20 smart contracts is in `./resources/tokens.json`. symbol and address are needed for each token, for instance:

```
{
  "symbol": "ACA",
  "address": "0x0000000000000000000000000000000001000000"
}
```


# Predeployed System Contracts

## ERC20 Contracts
These ERC20 contracts make native and cross-chain tokens available inside Acala EVM.
- ACA contract address: `0x0000000000000000000000000000000001000000`.
- AUSD contract address: `0x0000000000000000000000000000000001000001`.
- DOT contract address: `0x0000000000000000000000000000000001000002`.
- LDOT contract address: `0x0000000000000000000000000000000001000003`.
- RENBTC contract address: `0x0000000000000000000000000000000001000004`.

- KAR contract address: `0x0000000000000000000000000000000001000080`.
- KUSD contract address: `0x0000000000000000000000000000000001000081`.
- KSM contract address: `0x0000000000000000000000000000000001000082`.
- LKSM contract address: `0x0000000000000000000000000000000001000083`.

- LP_ACA_AUSD contract address: `0x0000000000000000000000010000000000000001`.
- LP_DOT_AUSD contract address: `0x0000000000000000000000010000000200000001`.
- LP_LDOT_AUSD contract address: `0x0000000000000000000000010000000300000001`.
- LP_RENBTC_AUSD contract address: `0x0000000000000000000000010000000400000001`.
- LP_KAR_AUSD contract address: `0x0000000000000000000000010000008000000081`.
- LP_KSM_AUSD contract address: `0x0000000000000000000000010000008200000081`.
- LP_LKSM_AUSD contract address: `0x0000000000000000000000010000008300000081`.
```
// Returns the currencyId of the token.
function currencyId() public view returns (uint256);

// Returns the name of the token.
function name() public view returns (string memory);

// Returns the symbol of the token, usually a shorter version of the name.
function symbol() public view returns (string memory);

// Returns the number of decimals used to get its user representation.
function decimals() public view returns (uint8);

// Returns the amount of tokens in existence.
function totalSupply() public view returns (uint256);

// Returns the amount of tokens owned by `account`.
function balanceOf(address account) public view returns (uint256);

// Moves `amount` tokens from the caller's account to `recipient`.
// Returns a boolean value indicating whether the operation succeeded.
// Emits a {Transfer} event.
function transfer(address recipient, uint256 amount) public returns (bool);

// Returns the remaining number of tokens that `spender` will be allowed to spend on behalf of `owner` through {transferFrom}.
// This is zero by default.
function allowance(address owner, address spender) public view returns (uint256);

// Sets `amount` as the allowance of `spender` over the caller's tokens.
// Returns a boolean value indicating whether the operation succeeded.
function approve(address spender, uint256 amount) public returns (bool);

// Moves `amount` tokens from `sender` to `recipient` using the allowance mechanism. `amount` is then deducted from the caller's allowance.
// Returns a boolean value indicating whether the operation succeeded.
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool);

// Atomically increases the allowance granted to `spender` by the caller.
// This is an alternative to {approve} that can be used as a mitigation for problems described in {IERC20-approve}.
// Emits an {Approval} event indicating the updated allowance.
function increaseAllowance(address spender, uint256 addedValue) public returns (bool);

// Atomically decreases the allowance granted to `spender` by the caller.
// This is an alternative to {approve} that can be used as a mitigation for problems described in {IERC20-approve}.
// Emits an {Approval} event indicating the updated allowance.
function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool);
```


## Other System Contracts:
These contracts make other chain-native functionalities available in Acala EVM.

### State Rent
- StateRent contract address: `0x0000000000000000000000000000000000000801`
```
// Returns the const of NewContractExtraBytes.
function newContractExtraBytes() public view returns (uint256);

// Returns the const of StorageDepositPerByte.
function storageDepositPerByte() public view returns (uint256);

// Returns the maintainer of the contract.
function maintainerOf(address contract_address) public view returns (address);

// Returns the const of DeveloperDeposit.
function developerDeposit() public view returns (uint256);

// Returns the const of DeploymentFee.
function deploymentFee() public view returns (uint256);

// Transfer the maintainer of the contract.
// Returns a boolean value indicating whether the operation succeeded.
function transferMaintainer(address contract_address, address new_maintainer) public returns (bool);
```

### Oracle Price Feed
- Oracle contract address: `0x0000000000000000000000000000000000000802`
```
// Get the price of the currency_id.
// Returns the (price, timestamp)
function getPrice(address token) public view returns (uint256, uint256);
```
### On-chain Automatic Scheduler
- ScheduleCall contract address: `0x0000000000000000000000000000000000000803`
```
// Schedule call the contract.
// Returns a boolean value indicating whether the operation succeeded.
function scheduleCall(address contract_address, uint256 value, uint256 gas_limit, uint256 storage_limit, uint256 min_delay, bytes memory input_data) public returns (bool);

// Cancel schedule call the contract.
// Returns a boolean value indicating whether the operation succeeded.
function cancelCall(bytes memory task_id) public returns (bool);

// Reschedule call the contract.
// Returns a boolean value indicating whether the operation succeeded.
function rescheduleCall(uint256 min_delay, bytes memory task_id) public returns (bool);
```

### DEX
- DEX contract address: `0x0000000000000000000000000000000000000804`
```
// Get liquidity of the currency_id_a and currency_id_b.
// Returns (liquidity_a, liquidity_b)
function getLiquidity(address tokenA, address tokenB) public view returns (uint256, uint256)

// Get swap target amount.
// Returns (target_amount)
function getSwapTargetAmount(address[] calldata path, uint256 supplyAmount) external view returns (uint256);

// Get swap supply amount.
// Returns (supply_amount)
function getSwapSupplyAmount(address[] calldata path, uint256 targetAmount) external view returns (uint256);

// Swap with exact supply.
// Returns a boolean value indicating whether the operation succeeded.
function swapWithExactSupply(address[] calldata path, uint256 supplyAmount, uint256 minTargetAmount) external returns (bool);

// Swap with exact target.
// Returns a boolean value indicating whether the operation succeeded.
function swapWithExactTarget(address[] calldata path, uint256 targetAmount, uint256 maxSupplyAmount) external returns (bool);

// Add liquidity to the trading pair.
// Returns a boolean value indicating whether the operation succeeded.
function addLiquidity(address tokenA, address tokenB, uint256 maxAmountA, uint256 maxAmountB) external returns (bool);

// Remove liquidity from the trading pair.
// Returns a boolean value indicating whether the operation succeeded.
function removeLiquidity(address tokenA, address tokenB, uint256 removeShare) external returns (bool);
```

## DeFi Contracts (Coming Soon)
These contracts will make Acala's DeFi primitives (stablecoin, staking derivative, and DeX) available in Acala EVM.
