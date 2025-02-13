# Qadir Adesoye VotingSystem Smart Contract

## Overview
The `VotingSystem` smart contract is a decentralized and secure election system built on Ethereum. It allows for anonymous voting using hashed votes, ensuring voter privacy while maintaining integrity. The contract enables the registration of voters, vote casting, and secure result verification.

## Features
- **Voter Registration**: Only the contract owner can register voters.
- **Secure Voting**: Voters submit hashed votes to maintain anonymity.
- **Non-Reentrancy Protection**: Ensures security against reentrancy attacks.
- **Emergency Pause**: The contract owner can pause and resume voting if necessary.
- **Vote Verification**: Allows checking if a vote was cast correctly.
- **Results Revealing**: After voting ends, results are revealed by the owner.
- **Voting Statistics**: Retrieve election details like total votes and timeframes.

## Contract Deployment
Deploy the contract by specifying the voting start time, end time, and candidate list:

```solidity
constructor(
    uint256 _votingStart,
    uint256 _votingEnd,
    uint256[] memory _candidates
) Ownable(msg.sender);
```

### Parameters:
- `_votingStart`: Unix timestamp for when voting begins.
- `_votingEnd`: Unix timestamp for when voting ends.
- `_candidates`: List of candidate IDs.

## Usage

### Register Voter
Only the owner can register eligible voters before voting begins:
```solidity
function registerVoter(address _voter) external onlyOwner whenNotPaused;
```

### Cast Vote
Registered voters submit a hashed vote during the voting period:
```solidity
function castVote(bytes32 _voteHash) external onlyDuringVoting whenNotPaused nonReentrant;
```

### Verify Vote
Check if a voter's hashed vote matches their submitted vote:
```solidity
function verifyVote(address _voter, bytes32 _voteHash) external view returns (bool);
```

### Reveal Results
After voting ends, the owner reveals the election results:
```solidity
function revealResults(address[] calldata _voters, uint256[] calldata _candidateChoices) external onlyOwner onlyAfterVoting whenNotPaused nonReentrant;
```

### Get Election Results
Retrieve final vote counts for each candidate:
```solidity
function getResults() external view returns (uint256[] memory, uint256[] memory);
```

### Get Voting Statistics
Retrieve total votes, start time, end time, and result status:
```solidity
function getVotingStats() external view returns (uint256, uint256, uint256, uint256, bool);
```

### Emergency Pause
The contract owner can pause or unpause the contract in case of an emergency:
```solidity
function emergencyPause() external onlyOwner;
function emergencyUnpause() external onlyOwner;
```

## Security Measures
- **Reentrancy Guard**: Prevents reentrancy attacks in sensitive functions.
- **Hashed Votes**: Ensures voter anonymity and prevents vote manipulation.
- **Access Control**: Only the owner can register voters and reveal results.
- **Paused State**: Allows contract suspension in emergencies.

## Events
- `VoterRegistered(address indexed voter)` - Logs when a voter is registered.
- `VoteCast(address indexed voter, bytes32 voteHash)` - Logs a casted vote.
- `ResultsRevealed(uint256 timestamp)` - Logs when results are revealed.
- `EmergencyPause(bool paused)` - Logs emergency pause status.
- `VoteVerified(address indexed voter)` - Logs when a vote is verified.

## Notes
- Voters must be registered before voting starts.
- The contract owner is responsible for revealing results.
- Once results are revealed, votes cannot be altered.

## License
This project is licensed under the MIT License.

