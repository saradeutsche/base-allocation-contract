# base-allocation-contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BaseAllocation {
    address public owner;
    mapping(address => uint256) public allocations;
    
    event Allocated(address indexed recipient, uint256 amount);
    event Withdrawn(address indexed recipient, uint256 amount);
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not authorized");
        _;
    }
    
    constructor() {
        owner = msg.sender;
    }
    
    function allocate(address recipient, uint256 amount) external onlyOwner {
        require(recipient != address(0), "Invalid recipient");
        require(amount > 0, "Amount must be greater than zero");
        allocations[recipient] += amount;
        emit Allocated(recipient, amount);
    }
    
    function withdraw() external {
        uint256 amount = allocations[msg.sender];
        require(amount > 0, "No funds allocated");
        
        allocations[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
        emit Withdrawn(msg.sender, amount);
    }
    
    receive() external payable {}
}
gtyhhj
