Retirement Fund

even contracts which aren't "payable" can still be forced to receive ether via the `selfdestruct()` function. A design choice of the EVM, smart contracts are incapable of refusing ETH sent to them as a result of the `SUICIDE` opcode.

The idea is that a penalty occurred if the start balance is less than the current balance. However, this logic does not work in Ethereum because we can always force send ether to a contract. Usually, sending ether to a contract requires a fallback function to be implemented, but one can force-send ether by calling the selfdestruct instruction on a contract containing ether. This instruction bypasses any checks:

    The attacker can do this by creating a contract, funding it with 1 wei, and invoking selfdestruct(victimAddress). No code is invoked in victimAddress, so it cannot be prevented. This is also true for block reward which is sent to the address of the miner, which can be any arbitrary address. Also, since contract addresses can be precomputed, ether can be sent to an address before the contract is deployed. - ConsenSys - Secure Development Recommendations

In essence, if we could force some ethers into the contract, making address(this).balance > startBalance prompting an overflow to variable withdraw, we will be able to drain every ether within this contract. The two ways of doing so is well documented in the Solidity docs:

https://docs.soliditylang.org/en/latest/contracts.html


