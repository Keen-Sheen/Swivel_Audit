# Swivle_Audit

## Audit of Swivle

Below is a "Call Graph" of the Swivel Token. This call graph shows the `LibFuse.sol` file which has the `block.number` contained within it.

![A Call Graph of LibFuse](ICERC20_Graph.svg)


--------------------------------------------------


## Potential use of "block.number" as source of randonmness.

  * The environment variable `block.number` looks like it could be used as a source of randomness. The values of variables like coinbase, gaslimit, block number and timestamp are predictable and can be manipulated by a malicious miner. Also keep in mind that attackers know hashes of earlier blocks. Developers shouldnt use any of those environment variables as sources of randomness and should also be aware that the use of these variables introduces a certain level of trust into miners.


-----------------------------------------------------

## Weak Sources of Randomness from Chain Attributes [SWC-120](https://swcregistry.io/docs/SWC-120)
## Use of Insufficiently Random Values


https://github.com/Keen-Sheen/Swivel_Audit/blob/5eb11ab79ac3bcbe1165d2fcbb6062e42d7ac5b7/2022-07-swivel/Tokens/LibFuse.sol#L19

https://github.com/Keen-Sheen/Swivel_Audit/blob/0ba71e0233714ff98fba5a152dafd6c0953c01b0/2022-07-swivel/Tokens/LibFuse.sol#L38


------------------------------------------------------


## Incorrect Inheritance Order

* Solidity supports multiple inheritance, meaning that one contract can inherit several contracts. Multiple inheritance introduces ambiguity called Diamond Problem. If two or more base contracts define the same function, which one should be called in the child contract? Solidity deals with this ambiguity by using reverse C3 Linearization, which sets a priority between base contracts.

That way, base contracts have different priorities, so the order of inheritance matters. Neglecting inheritance order can lead to unexpected behavior

![A Call Graph of Interfaces.sol](Inheritance_Graph.svg)

## Interface cannot inherit [SWC-125](https://swcregistry.io/docs/SWC-125)
## Incorrect Behavior Order


https://github.com/Keen-Sheen/Swivel_Audit/blob/dbf3c46aeca86fbdd337d8fa7ca8cd62fe9115fc/2022-07-swivel/Tokens/Interfaces.sol#L35

--------------------------------------------------------

## Prevention 

* Using commitment scheme, e.g. RANDAO.

* Using external sources of randomness via oracles, e.g. Oraclize. Note that this approach requires trusting in oracle, thus it may be reasonable to use multiple oracles.

* When inheriting multiple contracts, especially if they have identical functions, a developer should carefully specify inheritance in the correct order. The rule of thumb is to inherit contracts from general to more specific contracts.
