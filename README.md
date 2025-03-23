# 🚀 Bitcoin Regtest Guide

This guide walks you through setting up Bitcoin Core in **Regtest Mode** and running Python scripts to interact with your local Bitcoin node.

👥 Team Members

Vansh Khandelwal - Roll No: 230041038

Sameer Choudhary - Roll No: 230001070

Jai Pannu - Roll No: 230004019

---

## 📌 Prerequisites

- macOS (or Linux with slight modifications)
- Python 3 installed
- Homebrew installed (for package management)

---

## 🛠 Step 1: Install Bitcoin Core

### 🔹 For macOS
1. **Download Bitcoin Core** from the official website:  
   👉 [Bitcoin Core Download](https://bitcoincore.org/en/download/)
2. **Install Bitcoin Core**:
   - Open the `.dmg` file and drag `Bitcoin Core` into the Applications folder.
3. **Verify Installation**:
   ```bash
   bitcoin-cli --version
   ```
   If installed correctly, it will display the version.

---

## 🛠 Step 2: Configure Bitcoin Core for Regtest Mode

### 🔹 Create a Configuration File
Bitcoin Core needs a `bitcoin.conf` file to enable `regtest` mode.

1. **Create a Bitcoin data directory:**
   ```bash
   mkdir -p ~/Library/Application\ Support/Bitcoin
   ```
2. **Create and edit `bitcoin.conf`:**
   ```bash
   nano ~/Library/Application\ Support/Bitcoin/bitcoin.conf
   ```
3. **Paste the following configuration and save:**
   ```ini
   regtest=1
   server=1
   txindex=1
   rpcuser=RPCuser(Enter your username and update that in .env file)
   rpcpassword=RPCpassword(Enter your password and update that in .env file)
   rpcallowip=127.0.0.1
   rpcport=18443
   fallbackfee=0.0002
   mintxfee=0.00001
   txconfirmtarget=1
   ```

---

## 🛠 Step 3: Start Bitcoin Core in Regtest Mode

Start `bitcoind` in **regtest mode**:
```bash
bitcoind -regtest -daemon
```

### 🔹 Check if Bitcoin Core is Running
```bash
bitcoin-cli -regtest getblockchaininfo
```
If successful, you will see blockchain details.

---

## 🛠 Step 4: Set Up Python Virtual Environment

### 🔹 Create and Activate a Virtual Environment
1. **Navigate to your project directory:**
   ```bash
   cd ~/Desktop/bitcoin
   ```
2. **Create a virtual environment:**
   ```bash
   python3 -m venv venv
   ```
3. **Activate the virtual environment:**
   ```bash
   source venv/bin/activate  # macOS/Linux
   venv\Scripts\activate     # Windows
   ```
4. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

---

## 🛠 Step 5: Generate Initial Blocks
Regtest starts with **0 blocks**, so you need to **mine some**:
```bash
bitcoin-cli -regtest generatetoaddress 101 $(bitcoin-cli -regtest getnewaddress)
```
✅ This **creates 101 blocks** and provides BTC for testing.

---

## 🛠 Step 6: Run Your Python Scripts

### 🔹 Run the First Script (`task1.py`)
This script:
✔️ Connects to `bitcoind`  
✔️ Creates/loads a wallet  
✔️ Generates 3 addresses (A, B, C)  
✔️ Funds address A  
✔️ Sends BTC from A → B  

```bash
python -u "task1.py"
```

📌 **If you get errors:**
- Ensure `bitcoind` is running (`bitcoin-cli -regtest getblockchaininfo`)
- Check `bitcoin.conf` for correct `rpcuser` and `rpcpassword`
- If wallet errors occur, create a wallet:
  ```bash
  bitcoin-cli -regtest createwallet "test_wallet"
  ```

### 🔹 Run the Second Script (`task2.py`)
This script:
✔️ Fetches UTXOs from B  
✔️ Creates a transaction from B → C  
✔️ Decodes & analyzes the transaction  

```bash
python -u "task2.py"
```
### 🔹 Run the third Script (`task3.py`)
This script demonstrates a SegWit transaction by creating P2SH-SegWit addresses and executing transactions using them. It differs from the legacy transactions in that it:

- Generates SegWit addresses (p2sh-segwit) for addresses A', B', and C'
- Funds Address A'
- Creates a transaction from A' to B' and then from B' to C'
- Displays detailed information about the locking and unlocking scripts of the SegWit transaction

Run the SegWit script with:

```bash
python -u "task3.py"
```

---


## 🛠 Step 7: Debug Bitcoin Transactions

### 🔹 Install `btcdeb`
```bash
brew install bitcoin
```
or compile from source:
```bash
git clone https://github.com/bitcoin-core/btcdeb.git
cd btcdeb
make
```

### 🔹 Debug a transaction
```bash
btcdeb -v -s "<signature><public_key> OP_DUP OP_HASH160<recipient_public_key_hash>OP_EQUALVERIFYOP_CHECKSIG"
```

---

## 🎯 Summary
✅ **Step 1**: Install Bitcoin Core  
✅ **Step 2**: Configure `bitcoin.conf`  
✅ **Step 3**: Start `bitcoind`  
✅ **Step 4**: Create & activate virtual environment  
✅ **Step 5**: Mine initial blocks  
✅ **Step 6**: Run Python scripts  
✅ **Step 7**: Debug Bitcoin transactions  

Now you can successfully **run and debug Bitcoin transactions in Regtest mode!** 🚀

