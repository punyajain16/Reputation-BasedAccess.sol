
# 🛡️ ZKP-Based NFT (Zero-Input Solidity Contract)

A minimal **Zero-Knowledge Proof (ZKP) verified NFT smart contract** built entirely in **Solidity**, featuring:

- 🚫 **No imports**
- 🚫 **No constructors**
- 🚫 **No declared input parameters**
- 🧠 **ZKP-style verification flow using raw calldata**
- 🎨 **ERC-721-like NFT minting for verified users**

---

## 🔗 Deployed By
**Author:** [Punya Jain](https://github.com/PunyaJain)  
**Ethereum Address:** `0x818df1B6517D88D52Ff66bD5d1628E486b66dff7`

---

## 📘 Overview

This project demonstrates how **verified users** can mint NFTs using a **Zero-Knowledge Proof mechanism** — without needing to import external libraries or define input parameters.

The contract uses raw `msg.data` for proof verification and metadata initialization, achieving a pure and minimalist Solidity design.

### ✨ Key Features
- Fully self-contained — no external dependencies or imports.  
- Admin initialization via `initAdmin()`.  
- ZKP verification (via `verifyProof`) uses user-provided calldata.  
- Mint verified NFTs directly using `mintVerified()`.  
- Minimal ERC-721 implementation (transfer, approve, balance, ownerOf).

---

## ⚙️ Contract Functions

| Function | Description | Inputs | Notes |
|-----------|--------------|--------|-------|
| `initAdmin()` | Initializes the deployer/admin (can only be called once) | None | Sets `owner = msg.sender` |
| `setVerifierRoot()` | Sets the verifier root/commitment | 32 bytes in calldata | No input parameters declared; calldata is read directly |
| `mintVerified()` | Mints a verified NFT if proof passes | Proof bytes in calldata | Verifies proof using `keccak256(user, proof)` vs stored root |
| `transferFrom()` | Standard ERC721 transfer | - | Simplified transfer function |
| `burn()` | Burns a token | - | Owner or approved operator only |

---

## 🔍 ZKP Verification Logic (Demo)

This is a **placeholder verifier**:
```solidity
bytes32 h = keccak256(abi.encodePacked(user, proof));
return (h == verifierRoot);
````

✅ Replace `verifyProof()` with an actual **zkSNARK/zk-STARK verifier** generated from your zero-knowledge circuit for production-grade verification.

---

## 🚀 Usage

### 1️⃣ Deploy

Deploy the contract normally using Remix, Hardhat, or Foundry.

### 2️⃣ Initialize Admin

Call:

```
initAdmin()
```

(First caller becomes admin.)

### 3️⃣ Set Verifier Root

To set the verification root (example root = `0xabc...`):

```
calldata = selector(setVerifierRoot) || abi.encode(root)
```

Example in ethers.js:

```js
const iface = new ethers.utils.Interface(["function setVerifierRoot()"]);
const selector = iface.getSighash("setVerifierRoot()");
const root = "0xabc..."; // 32-byte commitment
const data = selector + root.slice(2);
await signer.sendTransaction({ to: contract.address, data });
```

### 4️⃣ Mint Verified NFT

Call `mintVerified()` with proof bytes appended to calldata:

```js
const iface = new ethers.utils.Interface(["function mintVerified()"]);
const selector = iface.getSighash("mintVerified()");
const proof = "0x1234abcd..."; // arbitrary proof bytes
const data = selector + proof.slice(2);
await signer.sendTransaction({ to: contract.address, data });
```

If `keccak256(abi.encodePacked(user, proof)) == verifierRoot`, mint succeeds 🎉

---

## 🧩 Technical Stack

* **Language:** Solidity `^0.8.20`
* **Standard:** ERC-721 (simplified)
* **Verification:** Placeholder hash-based ZKP
* **Storage:** Minimal mappings for balances and ownership

---

## 📜 License

MIT License © 2025 [Punya Jain](https://github.com/PunyaJain)

---

### 💬 Contact

📧 Email: *[add your email if you want]*
🐦 Twitter: [@PunyaJain1608](https://x.com/PunyaJain1608)
🔗 Ethereum: `0x818df1B6517D88D52Ff66bD5d1628E486b66dff7`

---

⭐ **If you found this project interesting, give it a star on GitHub!**

```

---

Would you like me to add a **“Usage Example” section with actual Remix instructions and screenshots (Markdown-ready)** so that anyone visiting your repo can directly test it?
```
