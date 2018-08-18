pragma solidity ^0.4.24;

interface TokenERC20 {
	function totalSupply() external view returns (uint256);
	function balanceOf(address who) external view returns (uint256);
	function transfer(address to, uint256 value) external returns (bool);
}

contract preICO {
    uint public buyPrice;
    TokenERC20 public token;
	bool public stop;
	
	uint constant cap = 100000 * (10 ** 18);
	uint constant stopdate = XXXXXXXXX; // time (unix)
	address constant stopowner = //ethereum wallet;
	
    constructor(TokenERC20 _token) public {
        token = _token;
        buyPrice = //X;
		stop = false;
    }

    function () public payable {
		require(!stop);
		if (now > stopdate || token.totalSupply() - token.balanceOf(this) > cap) _end();
		else token.transfer(msg.sender, msg.value);
    }

    function _end() internal {
		stopowner.transfer(address(this).balance);
		token.transfer(stopowner, token.balanceOf(this));
		stopowner.transfer(address(this).balance);
        stop = true;
    }	
}
