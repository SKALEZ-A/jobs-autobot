
---

# 🚀 Web3 Jobs & Projects Discovery Bot (n8n Workflow)

## 👨‍💻 Skills Profile (`skills.md`)

```md
# My Web3 Skills
- Blockchain Developer
- Smart Contract Developer (Solidity, Rust, TON, Solana)
- Fullstack Developer (React, Node.js, Express, MongoDB, Next.js)
- DeFi Protocol Development (DEX, Staking, Yield Farming, Lending)
- NFT Development (ERC721, ERC1155, NFT Marketplaces)
- Telegram/Discord Bot Development
- Web3 Mini Apps (TON, Telegram, Wallet integrations)
- Community Manager / Moderator
- Technical Writing & Documentation
- Web3 Content Creator (Articles, Tutorials, Videos)
- Security & Contract Audits (Basic to Intermediate)
- Data Analytics (Dune, Trino SQL, Blockchain explorers)
- DAO Setup & Governance Tools
```

---

## ⚙️ Workflow Overview

This bot automatically fetches:

* **New Projects** (from CoinGecko, CoinMarketCap, DeFiLlama, Dexscreener, ICODrops, RootData, etc.)
* **Web3 Jobs** (from CryptoJobsList, Web3.career, Remote3.co, others via scraping)
* Filters them against your `skills.md`.
* Sends a **Telegram message** with job/project matches daily.

---

## 🛠️ Tools Required

* **n8n MCP (inside Cursor IDE)** – handles workflow automation.
* **HTTP Request Node** – for APIs (CoinGecko, CoinMarketCap, DeFiLlama, Dexscreener).
* **HTML Extract Node** (n8n has this built-in) – for scraping job boards.
* **Function Node** – for filtering job descriptions against `skills.md`.
* **Telegram Node** – for sending notifications.
* **Database / File Node** – optional, to save history (SQLite/JSON).

---

## 🔗 APIs to Use

* **CoinMarketCap** (you already have API key: 261b070a-0b16-4f31-8c3b-43019e4280be):
  `https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest`
* **CoinGecko**:

  * `https://pro-api.coingecko.com/api/v3/coins/list/new`
  * `https://pro-api.coingecko.com/api/v3/onchain/tokens/info_recently_updated`
* **DeFiLlama** (free):

  * `https://api.llama.fi/raises` (fundraising)
  * `https://api.llama.fi/protocols`
* **Dexscreener**:

  * `https://api.dexscreener.com/latest/dex/tokens`
* **Jobs (no APIs)**:

  * Use **n8n HTML Extract Node** or external scraper like **Apify**, **Puppeteer**, or **Playwright**.
  * Example sites: [cryptojobslist.com](https://cryptojobslist.com), [web3.career](https://web3.career).\, CryptocurrencyJobs.co, Remote3.co, Blocktribe, obs3.io, SHE256 Job Board, WEB3board.io, Pomp Crypto Jobs, Aworker.io, CoinList Jobs, Ethlance, Social3, HireVibes (search the internet for the ones without links attached so you can know the links directed to these jobs with the websites its from.) 

---

## 🔄 n8n Workflow (Node by Node)

### 1. **Trigger**

* **Cron Node** → Runs daily at 9:00 AM.

### 2. **Fetch Data**

* **HTTP Request Node (CoinMarketCap)** → Fetch newly listed tokens.
* **HTTP Request Node (CoinGecko)** → Fetch newly added tokens.
* **HTTP Request Node (DeFiLlama)** → Fetch recent fundraising projects.
* **HTTP Request Node (Dexscreener)** → Fetch new trading pairs.
* **HTML Extract Node (CryptoJobsList)** → Scrape new jobs.
* **HTML Extract Node (Web3.career)** → Scrape jobs.

### 3. **Merge Data**

* **Merge Node** → Combine all project/job data into a single dataset.

### 4. **Parse & Filter**

* **Function Node (Skills Match)**:

  * Load `skills.md`.
  * Compare each job/project description with skills.
  * If match > 50% → keep.

  Example pseudo-code:

  ```js
  const fs = require('fs');
  const skills = fs.readFileSync('skills.md', 'utf8').toLowerCase().split('\n');

  return items.filter(item => {
    let text = item.json.description.toLowerCase();
    let matches = skills.filter(skill => text.includes(skill.toLowerCase()));
    item.json.matchScore = matches.length;
    return matches.length >= 3; // keep if at least 3 skills matched
  });
  ```

### 5. **Formatter**

* **Set Node** → Format job/project details into readable text for Telegram.

  Example Output:

  ```
  🚀 New Project: MemeFi ($MEFI)
  TG: t.me/memefi | Twitter: twitter.com/memefi

  👨‍💻 Job Match: Solidity Engineer at XYZ
  Skills Matched: Solidity, Node.js
  Apply: cryptojobslist.com/job/xyz
  ```

### 6. **Send to Telegram**

* **Telegram Node** → Sends daily alert with matched jobs & projects.

### 7. **Optional Storage**

* **Database Node** → Store results for history/analytics.

---

## 🕸️ Web Scraping Integration

If sites don’t have APIs:

* Use **n8n “HTTP Request” + “HTML Extract” nodes**.
* If blocked, integrate **Apify actors** or **Playwright Puppeteer scripts** → n8n supports executing them.
* Example: Scrape job titles, links, and descriptions from CryptoJobsList’s HTML.

---

## ✅ Daily Flow Summary

1. Trigger daily.
2. Pull projects from APIs.
3. Scrape jobs from sites.
4. Merge + filter by your skills.
5. Send Telegram digest.

---

👉 With this, your IDE can generate the automation. You just need to plug in API keys, set up a Telegram bot via BotFather, and configure the HTML extract selectors for the job boards.

---

Would you like me to **expand this into an actual `.json` export of an n8n workflow** (so you can import it directly instead of building node by node), or would you prefer to keep it in `.md` instruction style for your AI IDE to build?
