🚀 CryptoBot

CryptoBot is a Telegram bot for tracking cryptocurrency prices, fetching the latest news, and converting currencies.

🌟 Features

📊 Get real-time cryptocurrency prices (BTC, ETH, etc.).

📰 View the latest cryptocurrency news.

💱 Convert cryptocurrencies to USD and other currencies.

🔧 Installation

Clone the repository:

git clone https://github.com/your-username/crypto-bot.git

Navigate to the project folder:

cd crypto-bot

Install dependencies:

pip install -r requirements.txt

⚙️ Configuration

Obtain API keys:

CoinMarketCap API

NewsAPI

Exchangerate API

Set your API keys in the code:

API_KEY = 'YOUR_COINMARKETCAP_API_KEY'
NEWS_API_KEY = 'YOUR_NEWSAPI_KEY'

Add your Telegram bot token:

bot = telebot.TeleBot('YOUR_TELEGRAM_BOT_TOKEN')

▶️ Usage

Run the bot with:

python bot.py

Available commands:

/start — Welcome message.

/price [SYMBOL] — Get the current price of a cryptocurrency.

/news — Fetch the latest cryptocurrency news.

/convert [AMOUNT] [CRYPTO] in [CURRENCY] — Convert a cryptocurrency to another currency.

📌 Example Usage

/price BTC → Get the price of BTC in USD.

/convert 1 BTC in EUR → Convert 1 BTC to EUR.



