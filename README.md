Remicoin Smart Contract
=============================

<img src="assets/logo.png" />

A smart contract for send and recieve digital payment in a fast,secure and reliable manner.

Install & Run
-------------

### Compilation:
```

git clone https://github.com/remicoin/remicoin-contract.git
cd remicoin-contract

var contractSource = "pragma solidity^0.4.2;contract ERC20Interface{function balanceOf(address _owner)constant returns(uint256 balance);function transfer(address _to,uint256 _value)returns(bool success);function transferFrom(address _from,address _to,uint256 _value)returns(bool success);function approve(address _spender,uint256 _value)returns(bool success);function allowance(address _owner,address _spender)constant returns(uint256 remaining);event Transfer(address indexed _from,address indexed _to,uint256 _value);event Approval(address indexed _owner,address indexed _spender,uint256 _value)}
contract Owner{address public owner;function Owner(){owner=msg.sender}
modifier onlyOwner(){if(msg.sender!=owner)throw;_}
function transferOwnership(address new_owner)onlyOwner{owner=new_owner}}
contract RemiCoin is ERC20Interface,Owner{string public name;string public symbol;uint8 public decimals;uint256 public totalSupply;mapping(address=>uint256)balances;mapping(address=>bool)public frozenAccount;mapping(address=>mapping(address=>uint256))allowed;event FrozenFunds(address target,bool frozen);function RemiCoin(uint256 initial_supply,string _name,string _symbol,uint8 _decimal){balances[msg.sender]=initial_supply;name=_name;symbol=_symbol;decimals=_decimal;totalSupply=initial_supply}
function balanceOf(address _owner)constant returns(uint256 balance){return balances[_owner]}
function transfer(address to,uint value)returns(bool success){if(frozenAccount[msg.sender])return!1;if(balances[msg.sender]<value)return!1;if(balances[to]+value<balances[to])return!1;balances[msg.sender]-=value;balances[to]+=value;Transfer(msg.sender,to,value);return!0}
function transferFrom(address from,address to,uint value)returns(bool success){if(frozenAccount[msg.sender])return!1;if(balances[from]<value)return!1;if(allowed[from][msg.sender]>=value)return!1;if(balances[to]+value<balances[to])return!1;balances[from]-=value;allowed[from][msg.sender]-=value;balances[to]+=value;Transfer(from,to,value);return!0}
function allowance(address _owner,address _spender)constant returns(uint256 remaining){return allowed[_owner][_spender]}
function approve(address _spender,uint256 _amount)returns(bool success){allowed[msg.sender][_spender]=_amount;Approval(msg.sender,_spender,_amount);return!0}
function mintToken(address target,uint256 mintedAmount)onlyOwner{balances[target]+=mintedAmount;totalSupply+=mintedAmount;Transfer(0,owner,mintedAmount);Transfer(owner,target,mintedAmount)}
function freezeAccount(address target,bool freeze)onlyOwner{frozenAccount[target]=freeze;FrozenFunds(target,freeze)}
function changeName(string _name)onlyOwner{name=_name}
function changeSymbol(string _symbol)onlyOwner{symbol=_symbol}
function changeDecimals(uint8 _decimals)onlyOwner{decimals=_decimals}}"

var contractCompiled = web3.eth.compile.solidity(contractSource)
```

### Deploy:
```

var contract = web3.eth.contract(contractCompiled.info.abiDefinition)

var contractDeployed = contract.new(8400000000000000, 'RemiCoin', 'RMC', 8, {data: contract.code, from: web3.eth.accounts[0], gas: 4700000})
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



