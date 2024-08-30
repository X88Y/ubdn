# UBD Network DeTrust Multisig

## Main Contracts
- **DeTrustMultisigModelRegistry** - storing available templates for creating multisig. Charge creation fee (or monitoring the conditions - available UBDN balance);  

- **UsersDeTrustMultisigRegistry** - storing the list of multisigs in which the address is a signatory or creator;
- **DeTrustMultisigFactory** - the contract will create new multisigs from templates (mimmal EIP-1167 proxy to template implementations);  

- **DeTrustMultisigOnchainModel_00** - a model for creating multisigs indicating the date and time of the start of the powers of each signatory;  
- **DeTrustMultisigOnchainModel_01** - a model for creating multisigs with a “quiet period” (no transactions of the creator of the multisig) - the powers of the signatory come at the end of this period;  
- **DeTrustMultisigOffchainModel_01** - a model for creating offchain (like Gnosis) multisig "quiet period". _During the implementation, the customer decided to implement onchain multisig models (meta transactions are created and signed in the contract). As a result, this model has been tested only partially._

[Detrust Proxy Creation Sequence Diagram](./proxyCraeteSequenceDiagram.md)
### Build
```shell
$ # First build
$ git clone git@gitlab.com:ubd2/ubdn-multisig.git
$ cd ubdn-multisig
$ git submodule update --init --recursive
```

```shell
$ forge build
```

### Test

To run tests in local sandbox first please insatll [foundry](https://book.getfoundry.sh/getting-started/installation)  
```shell
$ forge test
```

### Deployments 
#### Sepolia
```shell
$ forge script script/Deploy.s.sol:DeployScript --rpc-url sepolia  --account ttwo --sender 0xDDA2F2E159d2Ce413Bd0e1dF5988Ee7A803432E3 --broadcast --verify  --etherscan-api-key $ETHERSCAN_TOKEN

$ # Script for geting hash for staroge addresses
$ forge script script/GetStorageSlot.s.sol:GetStorageSlot

$ # Script for BalnceCheckerDeploy
$ forge script script/Deploy-BalanceChecker.s.sol:DeployScriptBalanceChecker --rpc-url sepolia  --account ttwo --sender 0xDDA2F2E159d2Ce413Bd0e1dF5988Ee7A803432E3 --broadcast --verify  --etherscan-api-key $ETHERSCAN_TOKEN

$ # Run for create factory onky 
$ forge  create --rpc-url sepolia  --account ttwo   --etherscan-api-key $ETHERSCAN_TOKEN  --verify  src/DeTrustMultisigFactory.sol:DeTrustMultisigFactory --constructor-args 0xB5C0efdEc9a5252A778D91724e8F02e87CB06400 0x815eb5679636B4FdD38cC5282E018730047f9b6c

$ # Sometimes need separate run this comand for verify if above fail
$ forge verify-contract 0x4A89a183Ac7BaC19830492654723Eed997B301d7  ./src/DeTrustMultisigFactory.sol:DeTrustMultisigFactory  --num-of-optimizations 200 --compiler-version 0.8.26 --etherscan-api-key ${ETHERSCAN_TOKEN} --chain 11155111 --constructor-args $(cast abi-encode "constructor(address modelReg, address userReg)" 0xB5C0efdEc9a5252A778D91724e8F02e87CB06400 0x815eb5679636B4FdD38cC5282E018730047f9b6c)

$ # userReg.setFactoryState(address(factory), true);
$ cast  send 0x815eb5679636B4FdD38cC5282E018730047f9b6c  "setFactoryState(address,bool)" "0x4A89a183Ac7BaC19830492654723Eed997B301d7" "true" --rpc-url sepolia  --account ttwo


$ # Run for create PromoManagerV0 
$ forge  create --rpc-url sepolia  --account ttwo   --etherscan-api-key $ETHERSCAN_TOKEN  --verify  src/PromoManagerV0.sol:PromoManagerV0

$ # Sometimes need separate run this comand for verify if above fail
$ forge verify-contract 0x8687d6038B1FEe4820052b9055668610a41921Fb  ./src/PromoManagerV0.sol:PromoManagerV0  --num-of-optimizations 200 --compiler-version 0.8.26 --etherscan-api-key ${ETHERSCAN_TOKEN} --chain 11155111 
```
#### Ethereum Mainnet
```shell
$ # Script for BalnceCheckerDeploy
$ forge script script/Deploy-BalanceChecker.s.sol:DeployScriptBalanceChecker --rpc-url mainnet  --account ubd_deployer --sender 0x71373aa15b6d537E70138A39709B50e32C3660Ec --broadcast --verify  --etherscan-api-key $ETHERSCAN_TOKEN

$ forge script script/Deploy-UBD.s.sol:DeployScriptUBD --rpc-url mainnet  --account ubd_deployer --sender 0x71373aa15b6d537E70138A39709B50e32C3660Ec --broadcast --verify  --etherscan-api-key $ETHERSCAN_TOKEN
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```

