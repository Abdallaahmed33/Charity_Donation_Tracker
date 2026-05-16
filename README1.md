# Charity Donation Tracker — System Documentation

## What Is This App?

A transparent, blockchain-based donation platform where every donation to a charity campaign is permanently recorded on Ethereum (Ganache local network) and can never be hidden or changed. Built with Python (web3.py) and Solidity.

---

## Project Structure

```
charity_donation_tracker/
│
├── contracts/
│   ├── CharityTracker.sol          ← Core Smart Contract
│   └── DonorCoin.sol               ← Custom ERC-20 (Donor Coin / DNC)
│
├── auto_setup.py                   ← Deploy contracts + seed data (run first)
├── web3_connection.py              ← Shared Web3 helper
├── transaction_sender.py           ← All blockchain write/read functions
├── terminal_app.py                 ← Main terminal application (run this)
├── admin_dashboard.py              ← Admin summary report
├── live_alert.py                   ← Background donation alerts
├── data_history_report.py          ← Campaign history report
├── balance_snapshot_exporter.py    ← Export balances to CSV
├── security_test.py                ← Automated security tests
├── ownership_transfer_test.py      ← Ownership transfer test
└── README.md                       ← This file
```

---

## Requirements

- Python 3.10+
- Ganache (desktop app or CLI) running on `http://127.0.0.1:7545`
- Install Python packages:

```bash
pip install web3 py-solc-x
```

---

## Step 1 — Start Ganache

Open Ganache and start a **Quickstart Workspace** on port **7545**.
Make sure you see at least 10 accounts with test ETH.

---

## Step 2 — Deploy the Contracts

Run the setup script **once** to deploy both contracts and seed fake data:

```bash
python auto_setup.py
```

This will:
- Install and use Solidity compiler 0.8.20
- Deploy `DonorCoin.sol` and `CharityTracker.sol`
- Create 5 sample charity campaigns
- Seed sample donations for testing
- Save contract addresses to `deployed_addresses.json`
- Save ABIs to `contract_abis.json`

---

## Step 3 — Run the Terminal App

```bash
python terminal_app.py
```

You will be asked for your **wallet address** (copy one from Ganache).
If you have not registered before, you will be prompted to enter a display name.
You will also be asked for your **private key** (click the key icon in Ganache — this is only for the local test network).

---

## Normal User — How to Use

After logging in, you will see this menu:

```
[1]  View all campaigns          → See all charity campaigns with goals and progress
[2]  Donate to a campaign        → Choose a campaign and enter ETH amount to donate
[3]  Check coin & ETH balance    → Type any address to see its DNC and ETH balances
[4]  View my activity history    → See all your own blockchain transactions
[5]  View any address activity   → Enter any address to inspect their history
[9]  Admin menu (password)       → Hidden admin controls (requires password)
[0]  Exit
```

### How to Donate

1. Select `[2] Donate to a campaign`
2. The list of campaigns will appear with progress bars
3. Type the campaign ID (e.g. `1`)
4. Type the ETH amount (e.g. `0.5`)
5. The donation is recorded permanently on the blockchain
6. You receive DNC reward tokens automatically

---

## Admin — How to Use

From the main menu, press `[9]` and enter the admin password: **admin123**

> ⚠️ Change this password by editing `ADMIN_PASSWORD` near the top of `terminal_app.py`

The admin panel:

```
[1]  Add new campaign            → Create a new charity campaign
[2]  Update campaign             → Change name / description / goal
[3]  Batch add campaigns         → Add multiple campaigns in one transaction
[4]  Mint Donor Coins            → Create and send DNC tokens to any address
[5]  Pause / Resume contract     → Emergency stop switch
[6]  Transfer ownership          → Give admin rights to another address
[7]  Withdraw campaign funds     → Send raised ETH to a recipient address
[0]  Back to user menu
```

> ⚠️ Only the admin wallet address can perform these operations on-chain. Any other address will be rejected by the smart contract.

---

## Running Other Scripts

### Admin Dashboard (system summary)
```bash
python admin_dashboard.py
```
Prints: total campaigns, campaign details, total DNC minted, total transactions, top 3 active users.

### Live Alert System (run in a separate terminal)
```bash
python live_alert.py
```
Prints `ALERT: A new donation just happened!` whenever a donation is detected.

### Data History Report
```bash
python data_history_report.py
```
Shows how much ETH each campaign has raised, number of donations, and progress bars.

### Balance Snapshot Exporter
```bash
python balance_snapshot_exporter.py
```
Saves a CSV file (`balance_snapshot_YYYYMMDD_HHMMSS.csv`) with columns:
`Account Address, Display Name, Donor Coin Balance, ETH Balance`

### Security Tests
```bash
python security_test.py
```
Runs automated tests to confirm normal users cannot do admin actions. Prints PASS/FAIL.

### Ownership Transfer Test
```bash
python ownership_transfer_test.py
```
Full test: adds campaign as admin → transfers ownership → confirms old admin is blocked → confirms new admin works → restores ownership.

---

## Smart Contract Features

| Feature | Description |
|---|---|
| `addCampaign` | Admin adds a charity campaign |
| `updateCampaign` | Admin updates campaign details |
| `batchAddCampaigns` | Admin adds multiple campaigns in one transaction |
| `batchUpdateCampaigns` | Admin updates multiple campaigns in one transaction |
| `registerUser` | User registers a display name once |
| `donate` | User donates ETH to a campaign and receives DNC rewards |
| `pause / resume` | Admin emergency stop switch |
| `transferOwnership` | Admin transfers admin rights to new address |
| `withdraw` | Admin withdraws raised ETH from a campaign |
| `getAdmin` | View current admin address |
| `getUserName` | View registered display name for any address |

---

## Donor Coin (DNC) — ERC-20

- **Name**: Donor Coin
- **Symbol**: DNC
- **Decimals**: 18
- Only the CharityTracker contract can mint new DNC tokens (as rewards for donations)
- Any user can transfer DNC to other addresses
- Reward rate: 100 DNC per wei donated (100 × 10¹⁸ DNC per ETH)

---

## Troubleshooting

| Problem | Solution |
|---|---|
| `Cannot connect to Ganache` | Make sure Ganache is running on port 7545 |
| `deployed_addresses.json not found` | Run `python auto_setup.py` first |
| `Only admin can call this` | You are using a non-admin account for an admin function |
| `Contract is paused` | Admin must resume the contract from the admin menu |
| `Already registered` | Each wallet can only register once |
| `Transaction failed` | Check your ETH balance and that Ganache is still running |

---

## Notes

- All data lives on the local Ganache blockchain. Restarting Ganache resets everything — run `auto_setup.py` again.
- The `wallet_keys.json` file stores your private keys locally for convenience. **Never share this file or use real ETH private keys.**
- To use a real Ethereum testnet (e.g. Sepolia), change `GANACHE_URL` in `web3_connection.py` to the testnet RPC URL and update account private keys accordingly.
