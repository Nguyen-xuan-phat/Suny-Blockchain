# Suny-Blockchain
Giao dịch và mua bán Bitcoin thông qua Blockchain
Suny luôn đặt uy tín lên hàng đầu 
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("My Token", "MTK") {
        _mint(msg.sender, initialSupply * 10 ** decimals());
    }
}
npm install @openzeppelin/contracts
npm install --save-dev hardhat
npx hardhat
// scripts/deploy.js
const hre = require("hardhat");

async function main() {
  const MyToken = await hre.ethers.getContractFactory("MyToken");
  const token = await MyToken.deploy(1000000); // 1 triệu token
  await token.deployed();
  console.log("Token deployed to:", token.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
npx hardhat run scripts/deploy.js --network sepolia
npx create-react-app my-token-app
cd my-token-app
npm install ethers
import React, { useEffect, useState } from "react";
import { ethers } from "ethers";

// 🔗 Replace bằng contract của bạn
const tokenAddress = "0xYourTokenAddress";
const tokenABI = [
  "function balanceOf(address owner) view returns (uint)",
  "function transfer(address to, uint amount) returns (bool)",
  "function name() view returns (string)",
  "function symbol() view returns (string)",
  "function decimals() view returns (uint8)"
];

function App() {
  const [walletAddress, setWalletAddress] = useState("");
  const [tokenName, setTokenName] = useState("");
  const [symbol, setSymbol] = useState("");
  const [balance, setBalance] = useState("0");
  const [receiver, setReceiver] = useState("");
  const [amount, setAmount] = useState("");

  const connectWallet = async () => {
    if (!window.ethereum) return alert("Install MetaMask!");
    const [account] = await window.ethereum.request({ method: "eth_requestAccounts" });
    setWalletAddress(account);
    await loadTokenInfo(account);
  };

  const loadTokenInfo = async (account) => {
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const token = new ethers.Contract(tokenAddress, tokenABI, provider);
    const [name, symbol, rawBalance, decimals] = await Promise.all([
      token.name(),
      token.symbol(),
      token.balanceOf(account),
      token.decimals()
    ]);
    setTokenName(name);
    setSymbol(symbol);
    setBalance(ethers.utils.formatUnits(rawBalance, decimals));
  };

  const sendToken = async () => {
    if (!walletAddress || !receiver || !amount) return;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const signer = provider.getSigner();
    const token = new ethers.Contract(tokenAddress, tokenABI, signer);
    const decimals = await token.decimals();
    const tx = await token.transfer(receiver, ethers.utils.parseUnits(amount, decimals));
    await tx.wait();
    alert("Gửi token thành công!");
    loadTokenInfo(walletAddress);
  };

  return (
    <div style={{ padding: 30 }}>
      <h2>🪙 Ứng dụng Token đơn giản</h2>
      {!walletAddress ? (
        <button onClick={connectWallet}>Kết nối ví Metamask</button>
      ) : (
        <div>
          <p><b>Ví:</b> {walletAddress}</p>
          <p><b>Token:</b> {tokenName} ({symbol})</p>
          <p><b>Số dư:</b> {balance} {symbol}</p>

          <h4>Gửi Token</h4>
          <input placeholder="Địa chỉ nhận" onChange={(e) => setReceiver(e.target.value)} /><br /><br />
          <input placeholder="Số lượng" onChange={(e) => setAmount(e.target.value)} /><br /><br />
          <button onClick={sendToken}>Gửi</button>
        </div>
      )}
    </div>
  );
}

export default App;
