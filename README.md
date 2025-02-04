# Trading Agent

## Description
The **Trading Agent** is an automated trading bot designed for autonomous trading on Solana-based tokens. It evaluates token data, filters opportunities based on predefined trading criteria, and uses AI for final trade decisions. The bot executes trades and posts real-time updates on Twitter, sharing details about purchases and sales, including profit margins.

This project is built on **Eliza** where it integrates with APIs like **BirdEye** and **Helius** to gather live token rates and utilizes **OpenAI** for AI-driven trade decision-making. 

---

## Trading Logic

### **For Purchasing:**
1. Fetch token data such as market cap, volume, and bonding progress.
2. Finalize token purchase if:
   - Market cap reaches **$10K on PumpFun** within 24 hours.
   - Bonding market cap is **50% of the total market cap**.
   - **Buying volume** is **twice** the **selling volume** in the last 4 hours.
3. Retrieve wallet balance using BirdEye API.
4. Use AI via LLM to determine the appropriate purchase amount based on collected data.
5. Store purchase data in the **pumpfundata** table.
6. **Check for buying opportunities every 3 hours.**
7. After purchasing a token, **tweet** on Twitter to announce the purchase.

### **For Selling:**
1. Retrieve stored purchase positions from the database.
2. Fetch the current token price.
3. Compare prices for each token and execute sales:
   - Sell **50%** of the amount at **2x profit**.
   - Sell **25%** at **4x profit**.
   - Sell **12.5%** at **8x profit**.
4. Update the token position in the **pumpfundata** table after partial or full sales.
5. Store sale details in the **pumpfunsales** table.
6. **Check for selling opportunities every hour.**
7. After selling a token, **tweet** on Twitter to announce the sale and profit gained.

---

## Setup & Installation

### **Requirements:**
- Install **node, npm & pnpm**.
- Setup a **Supabase Database**.
- Create the following tables:
  - **pumpfundata**: Stores purchase details.
  - **pumpfunsales**: Stores sales records.
- Enable **realtime** on both tables and set permissions for **select, insert, and update** to `onion`.

### **Installation Steps:**

1. Clone the repository:
   ```bash
   git clone https://github.com/elizaOS/eliza.git
   cd <repo-folder>
   ```
2. Set up environment variables in the `.env` file:
   ```env
   TWITTER_API_KEY=
   TWITTER_API_SECRET_KEY=
   TWITTER_ACCESS_TOKEN=
   TWITTER_ACCESS_TOKEN_SECRET=
   OPENAI_API_KEY=
   TOKEN_MINT_ADDRESS=So11111111111111111111111111111111111111112
   CALLSTATIC_API_KEY=  # Obtain from CallStatic API
   BIRDEYE_API_KEY=  # Obtain from BirdEye API
   WALLET_ADDRESS=
   SUPABASE_KEY=  # Obtain from Supabase settings
   WALLET_PRIVATE_KEY=
   SUPABASE_PUMPFUN_SALES_URL=  # Supabase URL for pumpfunsales
   SUPABASE_PUMPFUN_PURCHASE_URL=  # Supabase URL for pumpfundata
   ```
3. Run the project using Cargo:
   ```bash
   pnpm build
   pnpm start  --characters="path/to/your/character.json"
   ```

---

## Database Schema

### **pumpfundata Table**
| Column       | Type       | Description                  |
|--------------|------------|------------------------------|
| id           | int8       | Unique identifier            |
| created_at   | timestamptz| Timestamp of entry           |
| mint         | text       | Token mint address           |
| price        | float8     | Purchase price               |
| status       | text       | Trade status                 |
| amount       | float8     | Purchased amount             |
| trxid        | text       | Transaction ID               |

### **pumpfunsales Table**
| Column       | Type       | Description                  |
|--------------|------------|------------------------------|
| id           | int8       | Unique identifier            |
| created_at   | timestamptz| Timestamp of sale            |
| mint         | text       | Token mint address           |
| price        | float8     | Sale price                   |
| amount       | float8     | Sold amount                  |
| trxid        | text       | Transaction ID               |
| purchase_id  | int8       | Associated purchase ID        |

---

## Notes
- Ensure all API keys are correctly set.
- Supabase permissions must allow **select, insert, and update** operations.
- Use a wallet with a sufficient Solana balance.
- This bot tweets all buy and sell transactions via an integrated Twitter API.

## Project Summary
This project automates the trading of PumpFun tokens by leveraging live market data, AI assessments, and automated decision-making. The integration with Twitter and APIs like BirdEye, Helius, and Supabase enables seamless, data-driven trading operations.

