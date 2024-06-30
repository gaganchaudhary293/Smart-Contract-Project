Token Smart Contract
Overview
This project implements a simple ERC-20-like token using Solidity. The smart contract includes basic functionalities such as minting, transferring, and burning tokens. It also demonstrates the use of Solidity's require, assert, and revert statements for error handling and input validation.

Features
Minting: The owner can mint new tokens to any address.
Transferring: Users can transfer tokens to other addresses.
Burning: Users can burn their own tokens.
Error Handling: The contract demonstrates the use of require, assert, and revert statements.
Requirements
Solidity ^0.8.0
Node.js and npm
Truffle
Ganache CLI (for local development)
Remix IDE (optional, for web-based development)
Getting Started
Setting Up Locally
Install Node.js and npm: Download and install from Node.js.

Install Truffle:

npm install -g truffle
Install Ganache CLI:


npm install -g ganache-cli
Initialize a Truffle project:


mkdir my-token-project
cd my-token-project
truffle init
Create the smart contract:

In the contracts directory, create a new file named Token.sol and paste the smart contract code.
Write a migration script:

In the migrations directory, create a new file named 2_deploy_contracts.js and add the following code:
js
const Token = artifacts.require("Token");

module.exports = function(deployer) {
    const initialSupply = 1000000; // Set the initial token supply
    deployer.deploy(Token, initialSupply);
};
Configure Truffle:

Open the truffle-config.js file and configure the development network:
js
module.exports = {
    networks: {
        development: {
            host: "127.0.0.1",
            port: 8545,
            network_id: "*",
        },
    },
    compilers: {
        solc: {
            version: "0.8.0",
        },
    },
};
Start Ganache:

sh
ganache-cli
Deploy the smart contract:

sh
truffle migrate --network development
Interact with the smart contract:

sh
truffle console --network development
js
let tokenInstance = await Token.deployed();
let owner = await tokenInstance.owner();
let balance = await tokenInstance.balanceOf(owner);
console.log(balance.toString());

await tokenInstance.mint(owner, 1000);
balance = await tokenInstance.balanceOf(owner);
console.log(balance.toString());

let accounts = await web3.eth.getAccounts();
await tokenInstance.transfer(accounts[1], 500);
balance = await tokenInstance.balanceOf(accounts[1]);
console.log(balance.toString());
Using Remix IDE
Open Remix: Go to Remix IDE.

Create a new file: Name it Token.sol and paste the smart contract code.

Compile the contract:

Select the Solidity compiler version ^0.8.0.
Click "Compile Token.sol".
Deploy the contract:

Go to the "Deploy & Run Transactions" tab.
Set the "Environment" to "JavaScript VM (London)".
Ensure "Token" is selected in the "Contract" dropdown.
Set the initial supply (e.g., 1000000).
Click "Deploy".
Interact with the contract:

Use the deployed contract instance to call functions like balanceOf, mint, transfer, and burn.
Smart Contract Code
solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Token {
    string public name = "ExampleToken";
    string public symbol = "ETK";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;

    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 value);
    event Burn(address indexed from, uint256 value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    constructor(uint256 initialSupply) {
        owner = msg.sender;
        mint(owner, initialSupply);
    }

    function mint(address to, uint256 value) public onlyOwner {
        require(to != address(0), "Cannot mint to the zero address");
        require(value > 0, "Mint value must be greater than zero");

        totalSupply += value;
        balanceOf[to] += value;
        emit Mint(to, value);
    }

    function transfer(address to, uint256 value) public {
        require(to != address(0), "Cannot transfer to the zero address");
        require(value > 0, "Transfer value must be greater than zero");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
    }

    function burn(uint256 value) public {
        require(value > 0, "Burn value must be greater than zero");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        totalSupply -= value;
        emit Burn(msg.sender, value);
    }

    function assertExample(uint256 x, uint256 y) public pure returns (uint256) {
        assert(x != y);
        return x + y;
    }

    function revertExample(uint256 value) public pure returns (uint256) {
        if (value == 0) {
            revert("Value cannot be zero");
        }
        return value * 2;
    }

    function requireExample(uint256 number) public pure returns (uint256) {
        require(number > 10, "Number must be greater than 10");
        return number * 2;
    }
}
Explanation
State Variables
name: The name of the token.
symbol: The symbol of the token.
decimals: The number of decimal places for the token.
totalSupply: The total supply of tokens.
balanceOf: A mapping that stores the balance of each address.
owner: The owner of the contract.
Events
Transfer: Emitted when tokens are transferred.
Mint: Emitted when new tokens are minted.
Burn: Emitted when tokens are burned.
Modifiers
onlyOwner: Ensures that only the owner can call certain functions.
Functions
constructor: Initializes the contract with an initial supply of tokens and sets the owner.
mint: Allows the owner to mint new tokens to an address.
transfer: Allows users to transfer tokens to another address.
burn: Allows users to burn their own tokens.
assertExample: Demonstrates the use of assert.
revertExample: Demonstrates the use of revert.
requireExample: Demonstrates the use of require.
License
This project is licensed under the MIT License.

