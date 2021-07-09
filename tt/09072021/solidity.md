# [Solidity Tour](https://docs.soliditylang.org/en/v0.8.6/) (Part 1)

## [Introdution](https://docs.soliditylang.org/en/v0.8.6/introduction-to-smart-contracts.html)

Solidity là một ngôn ngữ hướng đối tượng, bậc cao dùng để hiện thực Smart Contract. Smart Contract là một chương trình có thể tương tác với các accounts bên trong mạng Ethereum.

Block trong Solidity được phân biệt bằng dấu ngoặc nhọn(curly-bracket language). Vì ngôn ngữ sinh sau đẻ muộn nên cú pháp của nó bị ảnh hưởng bởi C++, Python và Javascript. Solidity được thiết kế với mục đích chạy trên Ethereum Virtual Machine(EVM).

Solidity là ngôn ngữ kiểu tĩnh(static typed), hổ trợ kế thừa. Có các library và các kiểu phức tạp(complex) do user tự định nghĩa,...

Solidity dùng để tạo những smart contract có chức năng như voting, huy động vốn từ cộng đồng(crowdfunding), đấu giá mù??(blind autions) và ví đa chữ ký (multi-signature wallets).

## [Layout of a Solidity Source File](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html)

Source files có thể chứa nhiều định nghĩa contract, source imports, pragma directive và các định nghĩa cấu trúc, enum, function, error, và constant variable.

### [1.Pragma](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html#pragmas)

**_pragma_** là một keyword dùng để cho phép duy nhất một version compiler. Mỗi file chỉ có một pragma, vì vậy, cần phải add pragma đến tất cả file trong source để không bị lỗi nếu muốn compile. Import file không có nghĩ là file được import có thể lấy pragma của file chứa nó để nó compile được.

```solidity
    //Hello.sol
    pragma solidity ^0.6.12;
```

Đoạn code ở trên thực hiện việc pragma solidity compiler version 0.6.12;

```solidity
    //./contracts/World.sol

    contract World {
        string public message;
        constructor(string memory _message){
            message = _message;
        }
    }
```

```solidity
    //./contracts/Hello.sol
    pragma solidity ^0.6.12;

    import './contracts/World.sol

    contract Hello {
        constructor(){}
    }
```

Đoạn code trong file _Hello.sol_ có import smart contract _World.sol_. Code trong _World.sol_ không có **_pragma_** để chỉ ra version compiler. Tuy _World.sol_ được import trong môi trường có **_pragma_**, nhưng không thể sử dụng version compiler trong môi trường nó được gọi. Bắt buộc bản thân nó phải khai báo một **_pragam_** cho riêng mình.

Ngoải ra còn có [ABI Coder Pragma](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html#abi-coder-pragma) và [Experimental Pragma](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html#experimental-pragma)

### [2.Import orther Source Files](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html#importing-other-source-files)

#### 2.1 Syntax and Semantics

Solidity hổ trợ import statments để giúp module hóa project. Một project có thể có nhiều contract, và các contract có thể tương tác, sử dụng lẫn nhau. Import statement giúp dễ dàng gọi các smart contract cả bên trong lẫn bên ngoài project. Tuy nhiên, solidity không hổ trợ khái niệm default export.

Tại level global, có thể dùng import statement như sau:

```solidity
    import "filename";
```

<span style="color:red"> _filename_</span> : Đường dẫn của file smart contract cần import.

Statement trên sẽ import tất cả symbol global(các import trong "**filename**") từ "**filename**" vào trong smart contract call import statement với scope hiện tại là global. Theo doc thì form này không được welcome, vì nó có thể gây ra những lộn xôn trong namespace mà không đoán trước được (các import trong file được import có thấy conflic với file hiện tại). Để tốt hơn thì nên import và chỉ rõ ra symbol tên là gì, cần phải alias cho nó:

```solidity
    import * as symbolName from "filename";
```

Từ đấy dễ dàng gọi các symbol khác (global scope) bằng format <span style="color:red">symbolName.symbol </span>

Một format khác để import một moldule, kết quả giống với format bên trên:

```solidity
import "filename" as symbolName;
```

Nếu tên symbol bị đụng độ, có thể đổi tên trong khi import. Ví dụ bên dưới chúng ta có thể rename symbol1 và symbol2 thành symbolName1 và symbolName2.

```solidity
import {symbol1 as symbolName1, symbol2 as symbolName2 } from "filename"
```

### [3.Comments](https://docs.soliditylang.org/en/v0.8.6/layout-of-source-files.html#comments)

Single line comment (<span style="color:red"> // </span>) và multi-line comments(<span style="color:red"> /_..._/ </span>).

```solidity
//This is single line comment
/*
This is multi-line comment.
*/
```

Ngoài ra Solidity cũng cung cấp một loại comment khác là [NetSpec(Ethereum Natural Language Specification Format)](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html) comment. Comment này cung cấp một rich document cho một function: name arguments, type return variable,... NatSpec commnet dùng <span style="color:red"> /\*_..._/</span> hoặc <span style="color:red"> /// </span>.

```solidity
   /**
* @dev Move `amount` tokens from the caller's account ro recipient
* Here, you provide the address you want to send and the amount to transfer
* @param _to The address of the recipient
* @param _value The amount tokens to be transferred
* @return Whether the transfer was successful or not
*
* Emits a {Transfer} event.
*
**/
function transfer(address _to, uint256 _value) external returns (bool);
```

```solidity
   ///@dev Move `amount` tokens from the caller's account ro recipient
   ///Here, you provide the address you want to send and the amount to transfer
   ///@param _to The address of the recipient
   ///@param _value The amount tokens to be transferred
   ///@return Whether the transfer was successful or not
   ///Emits a {Transfer} event.
   function transfer(address _to, uint256 _value) external returns (bool);
```

## [Smart Contract Overview](https://docs.soliditylang.org/en/v0.8.6/structure-of-a-contract.html)

Chapter này sẽ giới thiệu cơ bản về cấu trúc smartcontract những gì có thể có. Ở đây không đi sâu vào concept, chapter này chỉ giới thiệu cách sử dụng và chức năng của nó. Solidity Tour Part 2 sẽ giới thiệu chi tiết về Contract và nội dung chỉ xoay quanh nó.

Contract trong Solidity tương tự như class trong ngôn ngữ hướng đối tượng khác (object-oriented languages). Mỗi contract sẽ có thể có: State Variables, Functions, Function Modifiers, Events,
Errors, Struct Types, Enum Types

Ngoài ra cũng có các loại contract khác như [librarys](https://docs.soliditylang.org/en/v0.8.6/contracts.html#libraries) or [interfaces](https://docs.soliditylang.org/en/v0.8.6/contracts.html#interfaces). Sẽ được giới thiệu ở part 2.

### [1.State Variables](https://docs.soliditylang.org/en/v0.8.6/structure-of-a-contract.html#state-variables)

Giá trị của state variables(Biến trạng thái) đươc cho lifecircle bằng bằng lifecircle của contract chứa nó.

```solidity
pragma solidity ^0.6.12;
contract SmartContract {
    uint256 public count; //State variable
}
```

### [2.Functions](https://docs.soliditylang.org/en/v0.8.6/structure-of-a-contract.html#functions)

Function ở solidity cũng giống như function ở các ngôn ngữ khác. Ở solidity định nghĩa function là một đơn vị thực thi trong contract. Chúng có thể được defined lẫn bên trong và bên ngoài contract.

```solidity
pragma solidity ^0.6.12;
contract SmartContract {
    uint256 public count;

    //Function is defined inside contract
    function getCount() public returns (uint256){
        return count;
    }
}

//Helper function is defined outside of a contract
function add(uint256 a, uint256 b) pure returns (uint256){
    return a + b;
}
```

Lời gọi hàm (Function Calls) có thể xảy ra ở bên trong hoặc bên ngoài và có nhiều level trong scope ở nhiều contract khác nhau. Nói đơn giản thì function calls có thể thực hiện trong chính contract định nghĩa nó, hoặc có thể gọi từ contract khác nếu contract định nghĩa function được import. Function calls có thể gọi ở nhiều scope miễn là nơi gọi có thể thấy được phạm vi của function.

### 3.Function Modifiers

Function Modifiers(Hàm chỉnh sửa ??) là công cụ để chỉnh sửa các hàm bằng cách khai báo. Đễ dễ hiểu, function modifiers giống như một format header cho nhiều hàm khác nhau, những function thường có thể chỉnh sửa từ function modifiers.

```solidity
pragma solidity ^0.6.12;
contract SC{
    address private owner;
    modifier onlyOwner(){
        require(msg.sender == owner, "Only owner can call this");
        _;
    }

    function mint(uint256 _amount) public onlyOwner returns (bool) {
        //...
    }
}
```

### 4.Events

Để đơn giản, events được xem như log ở những ngôn ngữ khác, khi một event được emit, EVM sẽ log event đó lại và lưu vào trong block.

```solidity
pragma solidity ^0.6.12;
contract ERC20 {
    event Approval(address index owner, address index spender, uint256 amount);

    function approve(address spender, uint256 amount){
        //...
        emit Approal(mgs.sender, spender, amount); //Trigger event
    }
}
```

### 5.Errors

Error cho phép định nghĩa các mô tả cho names và data trong các tình huống failure. Error có thể được sử dụng trong [revert statements](https://docs.soliditylang.org/en/v0.8.6/control-structures.html#revert-statement) hoặc dùng _require_ function . So với việc kiểm tra lỗi bằng cách dùng string để so sánh, error dễ sử dụng và chi phí rẻ hơn nhiều. Có thể dùng NatSpec để mô tả lỗi cho người dùng.

```solidity
pragma solidity ^0.8.4;

/// Not enough funds for transfer. Requested `requested`,
/// but only `available` available.
error NotEnoughFunds(uint requested, uint available);

contract Token {
    mapping(address => uint) balances;
    function transfer(address to, uint amount) public {
        uint balance = balances[msg.sender];
        if (balance < amount)
            revert NotEnoughFunds(amount, balance);
        balances[msg.sender] -= amount;
        balances[to] += amount;
        // ...
    }
}
```

```solidity
pragma solidity ^0.8.4;

contract Token {
    mapping(address => uint) balances;
    function transfer(address to, uint amount) public {
        uint balance = balances[msg.sender];
        require(balance >= amount, "Not Enough Funds")
        balances[msg.sender] -= amount;
        balances[to] += amount;
        // ...
    }
}
```

### 6.Struct Types

Structs là type được người dùng định nghĩa. Struct type có thể nhóm nhiều biến lại với nhau ([Strucs](https://docs.soliditylang.org/en/v0.8.6/types.html#structs) in type sections.)

```solidity
pragma solidity >=0.4.0 <0.9.0;
contract MasterChef{
    struct UserInfo {   //Struct type
        uint256 amount;
        uint256 rewardDebt;
    }
}
```

### 7.Enum Types

Cũng giống như enum ở các ngôn ngữ khác. Enum type trong solidity là tập hợp hữu hạn constant values được định nghĩa trong một enum. ([Enums](https://docs.soliditylang.org/en/v0.8.6/types.html#enums) in type sections.)

```solidity
pragma solidity >=0.4.0 <0.9.0;

contract Purchase {
    enum State { Created, Locked, Inactive } // Enum
}
```

## Types

### 1.Value types

### 2.Reference Types

### 3.Mapping Types

### 4.Operators Involving LValues

### 5.Conversions between Elementary Types

### 6.Conversion between Literals and Elementary Types
