# Solidity Tour (Part 1)

## Introdution

Solidity là một ngôn ngữ hướng đối tượng, bậc cao dùng để hiện thực Smart Contract. Smart Contract là một chương trình có thể tương tác với các accounts bên trong mạng Ethereum.

Block trong Solidity được phân biệt bằng dấu ngoặc nhọn(curly-bracket language). Vì ngôn ngữ sinh sau đẻ muộn nên cú pháp của nó bị ảnh hưởng bởi C++, Python và Javascript. Solidity được thiết kế với mục đích chạy trên Ethereum Virtual Machine(EVM).

Solidity là ngôn ngữ kiểu tĩnh(static typed), hổ trợ kế thừa. Có các library và các kiểu phức tạp(complex) do user tự định nghĩa,...

Solidity dùng để tạo những smart contract có chức năng như voting, huy động vốn từ cộng đồng(crowdfunding), đấu giá mù??(blind autions) và ví đa chữ ký (multi-signature wallets).

## Layout of a Solìdity Source File

Source files có thể chứa nhiều định nghĩa contract, source imports, pragma directive và các định nghĩa cấu trúc, enum, function, error, và constant variable.

### 1.Pragmas

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

### 2.Import orther Source File

### 3.Comments

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
