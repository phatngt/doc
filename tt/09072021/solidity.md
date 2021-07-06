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

 Single line comment (<span style="color:red"> // </span>) và multi-line comments(<span style="color:red"> /*...*/ </span>).
 ```solidity
//This is single line comment
/*
This is multi-line comment.
*/
 ```
 Ngoài ra Solidity cũng cung cấp một loại comment khác là [NetSpec(Ethereum Natural Language Specification Format)](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html) comment. Comment này cung cấp một rich document cho một function: name arguments, type return variable,... NatSpec commnet dùng <span style="color:red"> /**...*/</span> hoặc  <span style="color:red"> /// </span>.

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
 /**
    ///@dev Move `amount` tokens from the caller's account ro recipient
    ///Here, you provide the address you want to send and the amount to transfer
    ///@param _to The address of the recipient
    ///@param _value The amount tokens to be transferred
    ///@return Whether the transfer was successful or not
    ///Emits a {Transfer} event.
    function transfer(address _to, uint256 _value) external returns (bool);
 ```

## Smart Contract Overview

### 1.State Variables

### 2.Functions

### 3.Function Modifiers

### 4.Events

### 5.Errors

### 6.Struct Types

### 7.Enum Types

## Types

### 1.Value types

### 2.Reference Types

### 3.Mapping Types

### 4.Operators Involving LValues

### 5.Conversions between Elementary Types

### 6.Conversion between Literals and Elementary Types
