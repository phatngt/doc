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
<!-- ## Types
Solidity là ngôn ngữ có kiểu static(tĩnh), mỗi biến(global or local) khi khai báo cần phải có một kiểu được chỉ rõ. Kiểu trong solidity bao gồm kiểu phần tử(level đơn vị) và kiểu phức tạp(bao gồm nhiều kiểu combined lại).

Ngoài ra, type cũng có thể tương tác với những biểu thức có chưa toán tử khác nhau.

Khái niệm _"undefined"_ or _"null"_ không tồn tại trong Solidity, nhưng mỗi kiểu sẽ có một giá trị mặc định khi khai báo. Để xử lý những giá trị không mong muốn, có thể dùng _revert_ function.
### 1.Value types
#### 1.1 Booleans
Kiểu luận lý, có 2 giá trị **true** or **false**.

**Operator**:

```js
! : negation.

&&: conjunction, "and".

||: disjunction, "or".

==: equality.

!=: inequality.
```

#### 1.2 Integers

<span style="color:red"> int</span> /<span style="color:red"> uint</span>:
Là kiểu integer có dấu và không dấu. Kiểu integer có nhiều size: <span style="color:red"> int8</span>, <span style="color:red"> int16</span>,
<span style="color:red"> uint256</span>.

##### **Operator**:
```python
Comparations: <=, <, ==, >=, >. (evalutate to bool).
Bit operators: &(and),|(or), ^(bitwisr exclusive or - xor), ~(bitwise negation).
...
```
[Reference](https://docs.soliditylang.org/en/v0.8.6/types.html#integers)

##### **Math operations**:
- [Comparisons](https://docs.soliditylang.org/en/v0.8.6/types.html#comparisons)
- [Bit operations](https://docs.soliditylang.org/en/v0.8.6/types.html#bit-operations): ~int256(0) == int256(-1)
- [Shifts](https://docs.soliditylang.org/en/v0.8.6/types.html#shifts): x << y == x * 2\*\*y, x >> y == x / 2\*\*y
- [Addition, Subtraction and Multiplication](https://docs.soliditylang.org/en/v0.8.6/types.html#addition-subtraction-and-multiplication): -x == (T(0)-x): T type of x.
- [Division](https://docs.soliditylang.org/en/v0.8.6/types.html#division): Division rounds towards zero.
- [Modulo](https://docs.soliditylang.org/en/v0.8.6/types.html#modulo): int256(5) % int256(2) == int256(1) ; int256(5) % int256(-2) == int256(1) ; int256(-5) % int256(2) == int256(-1) ; int256(-5) % int256(-2) == int256(-1);
- [Exponentiation](https://docs.soliditylang.org/en/v0.8.6/types.html#exponentiation): x\*\*3 == x\*x\*x ;
0\*\*0 == 1;

#### [1.3 Fixed Poit Numbers](https://docs.soliditylang.org/en/v0.8.6/types.html#fixed-point-numbers)
Fixed Point Numbers vẫn chưa hổ trợ đầy đủ cho Solidity.
#### [1.4 Address](https://docs.soliditylang.org/en/v0.8.6/types.html#address).

Kiểu address có 2 loại:

- <span style="color:red"> address</span>.: Được chứa trong 20 bytes(size of an ETH address)
- <span style="color:red"> address payable</span>.: Giống như <span style="color:red"> address</span>., ngoài ra có thêm 2 members <span style="color:red"> transfer</span>. và <span style="color:red"> send</span>.

<span style="color:red"> address payable</span> có thể send Ether, còn <span style="color:red"> address </span> thì không.

Có thể conversions from <span style="color:red"> address payable</span> to <span style="color:red"> address</span>, còn conversions from <span style="color:red"> address</span> to <span style="color:red"> address payable</span> phải thông qua <span style="color:red"> payable (\<address\>)</span>.


### 2.Reference Types

### 3.Mapping Types

### 4.Operators Involving LValues

### 5.Conversions between Elementary Types

### 6.Conversion between Literals and Elementary Types -->

## Solidity for MasterChef Smart Contract.
### Giới thiệu MasterChef
**MasterChef**, có vai trò như tên gọi của nó, vua đầu bếp trên nền tàng **Ethereum**. Tưởng tượng **DeFi** project có một quầy bar cao cấp, có một **MasterChef** trình độ 3 sao michelin, đối diện ông ta là một cái bàn vòng, được chia ra nhiều khu vực, mỗi khu vực có 1 màu riêng biệt (**Pool**) nơi các **Staker** có thể ngồi và thưởng thức **Reward Token**. Chỉ cần các S*taker chọn một màu (**Pool**) và ngồi xuống,chấp nhận(**Approve**) cho **MasterChef** thò tay vào túi quần và  lấy ra Token (**stake**) có màu tương ứng(**Stake Token**), bỏ lên bàn, nơi **MasterChef** có thể thấy được (**MasterChef giữ staked token**). Qua tháng năm,
các **Staker** sẽ được **MasterChef** phục vụ đồ ăn, đồ uống (Reward Token) liên tục miễn là có Token đã staked đến khi quầy bar đóng cửa(**Seed phase already completed**). Khi quầy bar đóng cửa, các **Staker** có thể lấy lại các Token mình đã bỏ ra(**withdraw**). À ngoài ra các **Staker** có thể chi thêm Token(**stake**) hoặc rút ra(**withdraw**) trong thời gian quầy bar hoạt động, mà nhớ phải ăn uống(**nhận Reward Token trong thời gian seeding pool**) trước khi stake hoặc withdraw nha.

### Những thứ cần có để tạo MasterChef Smart Contract.
#### Cấu trúc của Smart Contract.
Smart Contract như một class của các ngôn ngữ hướng đối tượng khác, thay keyword class bằng contract. Trong file cần khai báo version compile, và tạo một Contract với tên MasterChef.
```solidity
pragma solidity 0.6.12;
contract MasterChef{
    //...
}
```
#### Các biến cơ bản cần có.
 **uint256**: Biến integer có size 256 bit, range 0 <= x <= 2\*\*256 - 1. Dùng để lưu trữ Token value của user, các loại block, các biến tạm để thực hiện tính toán cho reward,...

 **string**: String trong Solidity cần phải khai báo keyword **memory** để chỉ định giá trị của biến string đó được lưu trong bộ nhớ tạm(~RAM). Trong MasterChef hiện tại chưa thấy sử dụng, nhưng có sử dụng trong Token SmartContract để tạo tham số **name** và **symbol** trong function **constructor**.

 **bool**: Như các ngôn ngữ khác, chỉ lưu 2 giá trị **true** và **false**.

 **address**: Dùng để lưu trữ địa chỉ của Token, User, Wallet,... Các giá trị địa chỉ được lưu trữ dưới dạng hexa,có độ dài mặc định 20 bits.

 **address payable**: Giống như **address** ở trên, ngoài ra có thêm 2 member(method??) là send và transfer.

**mapping**: Biến này sử dụng khá nhiều trong hầu hết các SmartContract, giống như tên gọi của nó, biến này thực hiện map một input x -> trả về một output y. Giống như object type trong các ngôn ngữ khác.

```solidity
contract MasterChef{
    mapping(address=>uint256) _balance;
    //Example: _balance giữ 1 giá trị 0x949188c22D801A011486501a1496e8272B2B0Ca8 => 2830000000000000000000
    function balanceOf(address owner) public view return (uint256){
        //owner =  0x949188c22D801A011486501a1496e8272B2B0Ca8
        return balance[owner]; //Return  2830000000000000000000
    }
}
```

**struct**: Giống như struct trong C, biến có kiểu này sẽ giữ thêm nhiều giá trị có kiểu có thể khác nhau bên trong nó. Vì struct là reference type, để lưu trữ giá trị có kiểu struct vào một biến, biến đó cần được khai báo có thêm keyword **storage** để biến đó lưu trữ dưới harddisk Để dễ hình dùng hãy xem đoạn code bên dứoi.

```solidity
contract MasterChef{
    struct UserInfo {
        address userAddress,
        uint256 amount,
        uint256 rewardDebt
    }

    mapping(uint8=> mapping(address => UserInfo) public userInfo;

    function getUserInfo(uint8 _pid, address _user) public view returns (UserInfo){
        UserInfo storage user = userInfo[_pid][_user]
        return user;
    }
}
```

**array**: Giống như **struct** và **Data location**, cũng là reference type. Array có fixed size, size là k , kiêu là T => T[k] và dynamic size => T[]. Ngoài ra, có thể cấp phát bộ nhớ trong quá trình thực thi bằng keyword **new**. Array Literal, Array Members,
[More details](https://docs.soliditylang.org/en/v0.5.3/types.html#arrays).

#### Keyword.

**memory** và **storage**: Giải thích memory và storage trong Solidity.
Để dễ hiểu thì storage và solidity hoạt động tương tự như đĩa cứng và bộ nhớ RAM trên máy tính. Memory trong Solidity chỉ lưu trữ dữ liệu tạm thời, trong khi đó Storage có thể giữ dữ liệu qua các lần gọi hàm khác nhau. Contract có thể sử dụng bất kỳ lượng bộ nhớ nào trong suốt quá trình thực thi, nhưng mỗi khi dừng, thì Memory sẽ bị xóa sạch cho lần đến lần thực thi kế tiếp. Trong khi đó, Storage lưu trữ liên tục, nghĩ là Contract có thể truy cập dữ liệu đã lưu trữ từ trước ở bộ nhớ.

Lưu ý: Gas sử  dụng cho Storage lớn hơn rất nhiều so với gas sử dụng Memory. Do đó, nên sử dụng memory để tính toán, cuối cùng lưu trữ kết quả cuối cùng trong Storage.

1.Biến trạng thái(State variables), biến cục bộ (Local Variable) của struct, mảng mặc định luôn được lưu trữ trong storage.

2.Function arguments là memory.

3.Khi create một instance mới của một mảng bằng keyword 'memory',một bảng coppy của mảng đó được tạo ra. Khi thay đổi giá trị trên instance mới, thì không ảnh hưởng đến giá trị của instance gôc.

Khi sử dụng một reference types(structs, arrays or mapping). Phải luôn cung cấp  rõ ràng vùng dữ liệu nơi mà chúng được lưu trữ:
*memory: Thời gian sống(lưu trữ) giới hạn bởi 1 lần gọi hàm.
*storage: Thời gian sống(lưu trữ) giới hạn bởi thời gian sống của contract.
*calldata: là nơi đặt biệt chỉ chứa đối số của các hàm, chỉ có cho các externel functions.

Giải thích keyword "memory" trong function constructor(string memory name, string memory symbol) trong Smart Contract BEP20.
Giải thích: Phiên bản 0.5.0 trở về sau có thể string, byte cũng là kiểu reference, compiler yêu cầu hàm trả về string cần phải có data location là memory or calldata và hàm có arguments type string cần phải có keyword memory (đối với external functions thì có thể là keyword calldata). Storage không được vì arguments có lifetime bằng lifetime của chính function đó.

**public**: tất cả đều có thể truy cập

**private**: chỉ duy nhất contract tạo nó mới có thể truy cập được variable, function private.

**external**: chỉ có thể access từ contract bên ngoài, không thể truy cập bên trong.
```solidity
pragma solidity 0.6.12;

contract Demo   {


      function getBlockTimestamp() external returns (uint256) {
        return block.timestamp;
    }

    function callBlockTimestamp() public view returns (uint256){
        uint256 timestamp = getBlockTimestamp();
        return timestamp;
    }
}
```

Result:

```bash
contracts/Test.sol:11:29: DeclarationError: Undeclared identifier. "getBlockTimestamp" is not (or not yet)
visible at this point. uint256 timestamp = getBlockTimestamp(); ^---------------^
```

**internal**: chỉ có thể access từ this contract và deriving contract. internal là default keywork của function.
```solidity
pragma solidity 0.6.12;

contract Demo {

    function getBlockTimestamp() internal view returns (uint256) {
        return block.timestamp;
    }

}
```

Result:

![Image result ](https://github.com/phatngt/doc/blob/main/tt/09072021/images/internal_result.png)


**view**: function có thể được view từ bên ngoài, có thể convert thành **non-payable** function.

**non-payable**: function từ chối Ether send đến nó, **non-payable** không thể convert thành **payable** function.

**payable**: function có thể gửi hoặc nhận Ether, nó cũng có thể là non-payable khi chấp nhận một payment là 0 Ether.

**returns**: thông báo kiểu trả về của function.

**event**: Tạo một event.

**emit**: Emit một event đã được tạo từ keyword **event**.

#### Modifier function
