Remicoin Token Smart Contract
=============================

A smart contract for send and recieve digital payment in a fast,secure and reliable manner.

Install & Run
-------------

### Compilation:
```

git clone https://github.com/remicoin/remicoin-contract.git
cd remicoin-contract

var contractSource = "pragma solidity^0.4.2;contract RemiCoin{string public name;string public symbol;uint8 public decimal;uint256 public totalSupply;mapping(address=>uint256)public balanceOf;event Transfer(address indexed from,address indexed to,uint256 value);function RemiCoin(uint256 initial_supply,string _name,string _symbol,uint8 _decimal){balanceOf[msg.sender]=initial_supply;name=_name;symbol=_symbol;decimal=_decimal;totalSupply=initial_supply;} function transfer(address to,uint value){if(balanceOf[msg.sender] < value)throw;if(balanceOf[to]+value < balanceOf[to])throw;balanceOf[msg.sender]-=value;balanceOf[to]+=value;Transfer(msg.sender,to,value);}}"

var contractCompiled = web3.eth.compile.solidity(contractSource)
```

### Deploy:
```

var contract = web3.eth.contract(contractCompiled.info.abiDefinition)

var contractDeployed = contract.new(24000000, 'Remicoin', 'RMC', 8,{data: contract.code, from: web3.eth.accounts[0], gas: 4700000})
```

### Interaction:
```

var contractInstance = contract.at(contractDeployed.address)

contractInstance.transfer.sendTransaction('toAddress','value', {
            from: fromAddress
            });
```

License
-------
All smart contracts are released under GPL v.3.



