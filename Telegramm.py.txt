import telebot
import requests
from collections import defaultdict


bot = telebot.TeleBot('6979024461:AAHCcgw-cBwkHpRZOLxsoyBOteXZpB48BT4')


API_KEY = 'afc256d4-014a-4e67-999c-af7dfd8e11c9'


NEWS_API_KEY = '98a2ad2e6b4b4f17b24c92dcae5bcaf6'


cache = defaultdict(dict)


def get_crypto_price(coin):
    if coin in cache['price']:
        return cache['price'][coin]
    url = f'https://pro-api.coinmarketcap.com/v1/cryptocurrency/quotes/latest'
    parameters = {
        'symbol': coin
    }
    headers = {
        'Accepts': 'application/json',
        'X-CMC_PRO_API_KEY': API_KEY,
    }
    response = requests.get(url, params=parameters, headers=headers)
    data = response.json()
    price = data['data'][coin]['quote']['USD']['price']
    cache['price'][coin] = price
    return price

def get_crypto_news():
    url = 'https://newsapi.org/v2/everything'
    parameters = {
        'q': 'crypto',
        'apiKey': NEWS_API_KEY,
        'language': 'en',
        'sortBy': 'publishedAt'
    }
    response = requests.get(url, params=parameters)
    data = response.json()
    articles = data['articles']
    return articles[:5]  


@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, "Привет! Я бот для отслеживания котировок криптовалют. Просто отправь мне название криптовалюты, например, BTC, ETH, и я скажу тебе их текущую цену в долларах.")


@bot.message_handler(commands=['news'])
def send_crypto_news(message):
    articles = get_crypto_news()
    for article in articles:
        bot.send_message(message.chat.id, f"{article['title']}\n{article['url']}")


@bot.message_handler(commands=['convert'])
def convert_crypto_currency(message):
    text = message.text.lower()
    try:
        parts = text.split()
        amount = float(parts[1])
        crypto_currency = parts[2]
        currency = parts[4]
        if currency.lower() == 'usd':
            price = get_crypto_price(crypto_currency.upper())
            converted_amount = amount * price
            bot.reply_to(message, f"{amount} {crypto_currency.upper()} в {currency.upper()}: {converted_amount:.2f}")
        else:
            url = f'https://api.exchangerate-api.com/v4/latest/USD'
            response = requests.get(url)
            data = response.json()
            exchange_rate = data['rates'][currency.upper()] 
            price = get_crypto_price(crypto_currency.upper())
            converted_amount = (amount * price) * exchange_rate
            bot.reply_to(message, f"{amount} {crypto_currency.upper()} в {currency.upper()}: {converted_amount:.2f}")
    except Exception as e:
        print(e)
        bot.reply_to(message, "К сожалению, не удалось выполнить операцию. Пожалуйста, убедитесь, что вы правильно ввели данные.")


@bot.message_handler(commands=['price'])
def send_crypto_price_command(message):
   
    coin = message.text.split()[1].upper()
    try:
       
        price = get_crypto_price(coin)
        bot.reply_to(message, f"Цена {coin}: ${price}")
    except:
        bot.reply_to(message, "К сожалению, не удалось получить информацию о криптовалюте. Пожалуйста, попробуйте еще раз.")

bot.polling()