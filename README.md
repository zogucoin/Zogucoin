
<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ZOGU Wallet Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/@solana/web3.js@latest/lib/index.iife.min.js"></script>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #0e0e0e;
      color: #f0f0f0;
    }
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem;
      background-color: #1c1c1c;
    }
    .hamburger {
      cursor: pointer;
      font-size: 24px;
    }
    .menu {
      display: none;
      position: absolute;
      top: 60px;
      right: 10px;
      background-color: #1e1e1e;
      padding: 1rem;
      border-radius: 8px;
    }
    .menu input {
      margin-bottom: 0.5rem;
      width: 100%;
      padding: 0.3rem;
      background: #111;
      border: 1px solid #333;
      color: #fff;
    }
    .wallet-info {
      padding: 2rem;
    }
    .token {
      margin: 1rem 0;
      font-size: 1.2rem;
    }
    button {
      padding: 0.6rem 1.2rem;
      font-size: 1rem;
      cursor: pointer;
      background-color: #3c3c3c;
      border: none;
      color: #fff;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <header>
    <h1>ZOGU Dashboard</h1>
    <div class="hamburger" onclick="toggleMenu()">☰</div>
  </header>  <div class="menu" id="hiddenMenu">
    <h3>Edit Balances</h3>
    <input id="fakeZogu" placeholder="Fake ZOGU" type="text" />
    <input id="fakeSol" placeholder="Fake SOL" type="text" />
    <input id="fakeBonk" placeholder="Fake BONK" type="text" />
    <input id="fakeUsdc" placeholder="Fake USDC" type="text" />
    <button onclick="applyFakeBalances()">Apply</button>
    <button onclick="resetBalances()">Reset</button>
  </div>  <div class="wallet-info">
    <button onclick="connectWallet()">Connect Wallet</button>
    <div class="token" id="walletAddress">Wallet: Not Connected</div>
    <div class="token" id="solBalance">SOL: -</div>
    <div class="token" id="zoguBalance">ZOGU: Coming Soon 🚀</div>
    <div class="token" id="bonkBalance">BONK: -</div>
    <div class="token" id="usdcBalance">USDC: -</div>
  </div>  <script>
    let connection = new solanaWeb3.Connection(solanaWeb3.clusterApiUrl('mainnet-beta'));
    let publicKey = null;

    const BONK_MINT = new solanaWeb3.PublicKey("DezXKX5xG8g7VjGzX1nGEsSMLzGtXhUsfTJdMMGz5YkD");
    const USDC_MINT = new solanaWeb3.PublicKey("Es9vMFrzaCER3Lk7Vi9aU0XD7Jmncfkb4wp7CXAdVfFv");

    async function connectWallet() {
      if (window.solana && window.solana.isPhantom) {
        try {
          const resp = await window.solana.connect();
          publicKey = resp.publicKey;
          document.getElementById("walletAddress").innerText = `Wallet: ${publicKey.toBase58()}`;
          getSolBalance();
          getTokenBalance(BONK_MINT, "bonkBalance");
          getTokenBalance(USDC_MINT, "usdcBalance");
        } catch (err) {
          alert("Wallet connection failed.");
        }
      } else {
        alert("Phantom Wallet not found.");
      }
    }

    async function getSolBalance() {
      const balance = await connection.getBalance(publicKey);
      document.getElementById("solBalance").innerText = `SOL: ${balance / solanaWeb3.LAMPORTS_PER_SOL}`;
    }

    async function getTokenBalance(mint, elementId) {
      const tokenAccounts = await connection.getParsedTokenAccountsByOwner(publicKey, {
        mint: mint,
      });
      let balance = 0;
      if (tokenAccounts.value.length > 0) {
        balance = tokenAccounts.value[0].account.data.parsed.info.tokenAmount.uiAmount;
      }
      document.getElementById(elementId).innerText = `${elementId.replace("Balance", "").toUpperCase()}: ${balance}`;
    }

    function toggleMenu() {
      const menu = document.getElementById("hiddenMenu");
      menu.style.display = menu.style.display === "block" ? "none" : "block";
    }

    function applyFakeBalances() {
      const zogu = document.getElementById("fakeZogu").value;
      const sol = document.getElementById("fakeSol").value;
      const bonk = document.getElementById("fakeBonk").value;
      const usdc = document.getElementById("fakeUsdc").value;

      if (zogu) document.getElementById("zoguBalance").innerText = `ZOGU: ${zogu}`;
      if (sol) document.getElementById("solBalance").innerText = `SOL: ${sol}`;
      if (bonk) document.getElementById("bonkBalance").innerText = `BONK: ${bonk}`;
      if (usdc) document.getElementById("usdcBalance").innerText = `USDC: ${usdc}`;
    }

    function resetBalances() {
      connectWallet();
      document.getElementById("zoguBalance").innerText = "ZOGU: Coming Soon 🚀";
    }
  </script></body>
</html>
<!--
**zogucoin/Zogucoin** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
