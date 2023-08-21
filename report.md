# Report

## Gas Optimizations

|                 | Issue                                                                                       | Instances |
| --------------- | :------------------------------------------------------------------------------------------ | :-------: |
| [GAS-1](#GAS-1) | Using bools for storage incurs overhead                                                     |     1     |
| [GAS-2](#GAS-2) | Cache array length outside of loop                                                          |     1     |
| [GAS-3](#GAS-3) | For Operations that will not overflow, you could use unchecked                              |    73     |
| [GAS-4](#GAS-4) | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) |     1     |
| [GAS-5](#GAS-5) | Using `private` rather than `public` for constants, saves gas                               |     2     |

### <a name="GAS-1"></a>[GAS-1] Using bools for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

_Instances (1)_:

```solidity
File: ProxyFactory.sol

71:     mapping(address => bool) public whitelistedTokens;

```

### <a name="GAS-2"></a>[GAS-2] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

_Instances (1)_:

```solidity
File: ProxyFactory.sol

85:         for (uint256 i; i < _whitelistedTokens.length;) {

```

### <a name="GAS-3"></a>[GAS-3] For Operations that will not overflow, you could use unchecked

_Instances (73)_:

```solidity
File: Distributor.sol

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

26: import {IERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

28: import {SafeERC20} from "../lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol";

29: import {ProxyFactory} from "./ProxyFactory.sol";

57:     uint8 private constant VERSION = 1; // version is 1 for now

57:     uint8 private constant VERSION = 1; // version is 1 for now

58:     address private immutable FACTORY_ADDRESS; //@audit immutable consistency

58:     address private immutable FACTORY_ADDRESS; //@audit immutable consistency

60:     uint256 private constant COMMISSION_FEE = 500; // this can be changed in the future

60:     uint256 private constant COMMISSION_FEE = 500; // this can be changed in the future

79:         if (factoryAddress == address(0) || stadiumAddress == address(0)) revert Distributor__NoZeroAddress(); // @audit if condition consistency, no brackets here, but brackets are present in other

79:         if (factoryAddress == address(0) || stadiumAddress == address(0)) revert Distributor__NoZeroAddress(); // @audit if condition consistency, no brackets here, but brackets are present in other

80:         FACTORY_ADDRESS = factoryAddress; // initialize with deployed factory address beforehand

80:         FACTORY_ADDRESS = factoryAddress; // initialize with deployed factory address beforehand

81:         STADIUM_ADDRESS = stadiumAddress; // official address to receive commission fee

81:         STADIUM_ADDRESS = stadiumAddress; // official address to receive commission fee

123:         if (token == address(0)) revert Distributor__NoZeroAddress(); // @audit if consistency

123:         if (token == address(0)) revert Distributor__NoZeroAddress(); // @audit if consistency

128:         if (winners.length == 0 || winners.length != percentages.length) revert Distributor__MismatchedArrays(); // @audit nested if checks

128:         if (winners.length == 0 || winners.length != percentages.length) revert Distributor__MismatchedArrays(); // @audit nested if checks

133:             totalPercentage += percentages[i]; // @audit += is used

133:             totalPercentage += percentages[i]; // @audit += is used

133:             totalPercentage += percentages[i]; // @audit += is used

133:             totalPercentage += percentages[i]; // @audit += is used

135:                 ++i;

135:                 ++i;

139:         if (totalPercentage != (10000 - COMMISSION_FEE)) {

147:         if (totalAmount == 0) revert Distributor__NoTokenToDistribute(); // @audit if consistency

147:         if (totalAmount == 0) revert Distributor__NoTokenToDistribute(); // @audit if consistency

149:         uint256 winnersLength = winners.length; // cache length

149:         uint256 winnersLength = winners.length; // cache length

152:             uint256 amount = totalAmount * percentages[i] / BASIS_POINTS;

152:             uint256 amount = totalAmount * percentages[i] / BASIS_POINTS;

155:                 ++i;

155:                 ++i;

```

```solidity
File: ProxyFactory.sol

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

26: import {Ownable} from "../lib/openzeppelin-contracts/contracts/access/Ownable.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

27: import {ECDSA} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

28: import {EIP712} from "../lib/openzeppelin-contracts/contracts/utils/cryptography/EIP712.sol";

29: import {Proxy} from "./Proxy.sol";

90:                 i++;

90:                 i++;

114:         if (closeTime > block.timestamp + MAX_CONTEST_PERIOD || closeTime < block.timestamp) {

199:         if (saltToCloseTime[salt] + EXPIRATION_TIME > block.timestamp) revert ProxyFactory__ContestIsNotExpired();

231:         if (saltToCloseTime[salt] + EXPIRATION_TIME > block.timestamp) revert ProxyFactory__ContestIsNotExpired();

```

### <a name="GAS-4"></a>[GAS-4] `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)

_Saves 5 gas per loop_

_Instances (1)_:

```solidity
File: ProxyFactory.sol

90:                 i++;

```

### <a name="GAS-5"></a>[GAS-5] Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_Instances (2)_:

```solidity
File: ProxyFactory.sol

64:     uint256 public constant EXPIRATION_TIME = 7 days;

65:     uint256 public constant MAX_CONTEST_PERIOD = 28 days;

```

## Low Issues

|             | Issue                                                                                                                       | Instances |
| ----------- | :-------------------------------------------------------------------------------------------------------------------------- | :-------: |
| [L-1](#L-1) | `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` |     1     |

### <a name="L-1"></a>[L-1] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`

Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).
If all arguments are strings and or bytes, `bytes.concat()` should be used instead

_Instances (1)_:

```solidity
File: ProxyFactory.sol

243:         bytes32 hash = keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256(code)));

```

## Medium Issues

|             | Issue                                  | Instances |
| ----------- | :------------------------------------- | :-------: |
| [M-1](#M-1) | Centralization Risk for trusted owners |     4     |

### <a name="M-1"></a>[M-1] Centralization Risk for trusted owners

#### Impact:

Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

_Instances (4)_:

```solidity
File: ProxyFactory.sol

37: contract ProxyFactory is Ownable, EIP712 {

82:     constructor(address[] memory _whitelistedTokens) EIP712("ProxyFactory", "1") Ownable() {

195:     ) public onlyOwner returns (address) {

224:     ) public onlyOwner {

```
