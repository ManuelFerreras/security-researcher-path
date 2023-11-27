# First Flight #2: Puppy Raffle
<br />

## Goals

- [ ] Check that duplicated addresses can only participate once.
<br>

- [ ] Check all variables are well used and unchanged variables remain immutable.
<br>

- [ ] Check the refund logic. Special attention to where the funds come from, conditions of refund, and that it is not reentrant.
<br>

- [ ] Check that fee address can only be set by the owner.
<br>

- [ ] Check that there are enough funds for any refunds, prices and fees.
<br>

## Notes

- Call the enterRaffle function with the following parameters:
  - address[] participants: A list of addresses that enter. You can use this to enter yourself multiple times, or yourself and a group of your friends.
<br>

- Duplicate addresses are not allowed
<br>

- Users are allowed to get a refund of their ticket & value if they call the refund function
<br>

- Every X seconds, the raffle will be able to draw a winner and be minted a random puppy
<br>

- The owner of the protocol will set a feeAddress to take a cut of the value, and the rest of the funds will be sent to the winner of the puppy.
<br>

## Variables Tree

```solidity
    uint256 public immutable entranceFee;

    address[] public players;
    uint256 public raffleDuration;
    uint256 public raffleStartTime;
    address public previousWinner;

    // We do some storage packing to save gas
    address public feeAddress;
    uint64 public totalFees = 0;

    // mappings to keep track of token traits
    mapping(uint256 => uint256) public tokenIdToRarity;
    mapping(uint256 => string) public rarityToUri;
    mapping(uint256 => string) public rarityToName;

    // Metadata
    string private commonImageUri = "ipfs://QmSsYRx3LpDAb1GZQm7zZ1AuHZjfbPkD6J7s9r41xu1mf8";
    uint256 public constant COMMON_RARITY = 70;
    string private constant COMMON = "common";

    string private rareImageUri = "ipfs://QmUPjADFGEKmfohdTaNcWhp7VGk26h5jXDA7v3VtTnTLcW";
    uint256 public constant RARE_RARITY = 25;
    string private constant RARE = "rare";

    string private legendaryImageUri = "ipfs://QmYx6GsYAKnNzZ9A6NvEKV9nf1VaDzJrqDR23Y8YSkebLU";
    uint256 public constant LEGENDARY_RARITY = 5;
    string private constant LEGENDARY = "legendary";
```

## Roles

- Owner: Deployer of the protocol, has the power to change the wallet address to which fees are sent through the changeFeeAddress function.
<br>

- Player: Participant of the raffle, has the power to enter the raffle with the enterRaffle function and refund value through refund function.
<br>

## Findings

- PuppyRaffle::raffleDuration can be set to immutable.
<br>

- PuppyRaffle::totalFees can have an overflow.
<br>

- PuppyRaffle::refund is a reentrant function.
<br>