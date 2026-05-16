# Charity Donation Tracker — React GUI

## Setup (one time)

1. Copy `deployed_addresses.json` and `contract_abis.json` from the main project into the `public/` folder:

```
copy ..\deployed_addresses.json public\
copy ..\contract_abis.json public\
```

2. Install dependencies:

```bash
npm install
```

3. Start the app:

```bash
npm start
```

The app opens at **http://localhost:3000**

---

## How to Use

- Open Ganache, copy a wallet address and its private key (🔑 icon)
- Paste them into the login screen
- The app connects directly to your local blockchain

### Pages

| Page | Description |
|---|---|
| Dashboard | Overview: campaigns, your balances, quick stats |
| Campaigns | All charity campaigns with progress bars |
| Donate | Pick a campaign, enter ETH amount, confirm donation |
| Balances | Look up any address's ETH + DNC balance |
| Activity | Scan blockchain for any address's history |
| Admin Panel | Add/update campaigns, mint coins, pause, withdraw (admin only) |

---

## Notes

- Make sure Ganache is running on port 7545 before starting
- Run `auto_setup.py` from the main project first to deploy contracts
- Admin Panel only appears if your wallet is the contract admin
