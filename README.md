# Lottery-8
 Lottery.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Lottery {
    address[] public players;
    address public winner;

    function enter() public payable {
        require(msg.value >= 0.01 ether, "Need 0.01 ETH to enter");
        players.push(msg.sender);
    }

    function pickWinner() public {
        require(players.length > 0, "No players");
        uint256 index = uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender))) % players.length;
        winner = players[index];
        payable(winner).transfer(address(this).balance);
        delete players;
    }
}
