# Creating a Standard ERC-721 token

This document showcases two standard implementations of the ERC-721 standard one: a personal implementation that follows Ethereum standard guidelines; the other is a standard implementation by OpenZeppelin. The ERC-721 standard can be used to build NFTs or non-fungible tokens on the Ethereum Blockchain.

In general, a Solidity smart contract is composed of multiple events and functions that allow for different actions to occur on the Blockchain. However, there are also what are now known to be conventional standards for implementing certain types of smart contracts, specifically for implementing the various ERC standards such as ERC-20 and, in this case, ERC-721.

- Implementation Standard 1 (Personal)
- Implementation Standard 2 (OpenZepplin)

## Implementation Standard 1 ERC-721 Functions

```sh
1. function approval(address, _owner, address _approved, uint _tokenId) —> approves the address to hold an NFT
2. function transfer(address _to, uint _amount) —> allows the transfer of tokens from one address to another
3. function balanceOf(address, _owner) —> tells us how many NFTs are in an address
4. function ownerOf(uint, _tokenId) —> tells us the address holding a specific NFT
5. function tranferFrom(address _from, address _to, uint _tokenId) —> transfers ownership of an NFT from one address to another address
6. function approve(address _approved, uint _tokenId) —> approves an address to hold an NFT
```

### Beginning of standard contract
```
pragma solidity ^0.5.0

contract NFT is ERC721 {
  mapping(address => uint) units;
  
  // FUNCTIONS AND EVENTS GO HERE

}
```

### 1. Approval
```
function approval(address, _owner, address _approved, uint _tokenId) {
  require(units[_owner] == _tokenId);
  units[_approved] = _tokenId;
}
```

### 2. Transfer
```
function transfer(address _to, uint _amount) public payable {
  // payable allows a function to receive ethereum while being called
  // public specifies the view (public functions can be called by anyone on the blockchain)
  
  require(_amount <= units[msg.sender]);
  units[msg.sender] -= amount;
  units[_to] += amount;
}
```

### 3. BalanceOf
```
function balanceOf(address, _owner) public view returns (uint) {
  return units[_owner];
}
```

### 4. OwnerOf
```
function ownerOf(uint, _tokenId) public view returns (address) {
  return units[_tokenId].address;
}
```

### 5. TransferFrom
```
function tranferFrom(address _from, address _to, uint _tokenId) payable {
  require(units[_from] == _tokenId);
  units[_from] = 0;
  units[_to] = _tokenId'
}
```

### 6. Approve
```
function approve(address _approved, uint _tokenId) payable {
  require(units[msg.sender] == _tokenId);
  units[_approved] = _tokenId;
}
```

## Implementation Standard 2 (OpenZeppelin)

This implementation comes directly from OpenZeppelin's documentation and can be found [here](https://docs.openzeppelin.com/contracts/3.x/erc721). This implementation provides an example of an ERC-721 contract being used to track items in a game. It's important to note ``` import "@openzeppelin/contracts/token/ERC721/ERC721.sol";``` contains much of the code from above. OpenZeppelin provides documentation for contract standards that have been put through a host of test suites.

```
pragma solidity ^0.6.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract GameItem is ERC721 {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() public ERC721("GameItem", "ITM") {}

    function awardItem(address player, string memory tokenURI)
        public
        returns (uint256)
    {
        _tokenIds.increment();

        uint256 newItemId = _tokenIds.current();
        _mint(player, newItemId);
        _setTokenURI(newItemId, tokenURI);

        return newItemId;
    }
}
```

