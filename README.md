# Both signatures needed

Demonstrates one way of implementing the constraint whereby 2 accounts are needed to validate the storage of a value. It makes use of `msg.data` and is implemented in a `modifier`.

```solidity
modifier isConfirmed {
    bytes32 key = hashData(msg.data);
    if (confirmations[key].confirmed[msg.sender]) {
        return;
    }
    confirmations[key].count++;
    confirmations[key].confirmed[msg.sender] = true;
    if (confirmations[key].count < 2) {
        OnUnfinishedConfirmation(key);
        return;
    }
    delete confirmations[key];
    _;
}
```

Tests require 4 accounts with ether.
