# ohstudioo_bot

import os
from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext

# Ответы по кнопкам
faq = {
    "📍 Где вы находитесь?": "📍 Адрес: Альметьевск, проспект Г. Тукая, 3Г. Вход через Озон, 3 этаж\nКарта: https://go.2gis.com/nk6rg",
    "📅 Как забронировать?": (
        "Для бронирования перейдите по ссылке:\nhttps://oh-studio.ru/order/1\n\n"
        "Там вы сможете:\n"
        "1. Посмотреть свободные даты и время 📅\n"
        "2. Узнать стоимость аренды и доп. услуг 🔍\n"
        "3. Забронировать зал ✒️\n"
        "4. Оплатить 💳\n\n"
        "Важно: укажите имя, фамилию, телефон (придет смс) и email."
    ),
    "🏠 Какие залы есть?": (
        "Зал 1 «Moss» (80м²): уютный интерьер, закатный свет, подходит для семейных и предметных съёмок\n"
        "Обзор: https://oh-studio.ru/moss\n\n"
        "Зал 2 «Wave» (142м²): большая циклорама, топовое оборудование, фэшн и корпоратив\n"
        "Обзор: https://oh-studio.ru/wave"
    ),
    "💸 Сколько стоит аренда?": "Цены зависят от зала и времени. Актуальные цены указаны при бронировании: https://oh-studio.ru/order/1",
    "📷 Что входит в аренду?": "Входит всё необходимое оборудование. Доп. оборудование можно запросить у администратора.",
    "👥 Сколько человек можно?": "В базовую аренду включено до 7 человек (включая детей, если они самостоятельно двигаются и персонал).",
    "💄 Есть ли грим-зона?": "Да, в каждом зале есть грим-зона с зеркалом и освещением.",
    "🔁 Перенести бронь": "Напишите нам в WhatsApp для переноса брони: https://wa.me/78432071309",
    "❌ Отменить бронь": "Чтобы отменить бронь, пожалуйста, свяжитесь с администратором по WhatsApp: https://wa.me/78432071309",
    "🎁 Как списать баллы?": "Условия списания и накопления баллов: https://oh-studio.ru/loyaltycard",
    "🧾 Добавить клубную карту": (
        "Добавьте карту по ссылке: http://oh-studio.ru/cards/1/add\n"
        "Она бесплатна. Добавьте её в Apple Wallet или Telegram за 1 минуту!"
    ),
    "📞 Позвонить вам": "📞 Телефон студии: +7 (843) 207-13-09\nЖдём вас!",
}

# Главное меню
main_keyboard = ReplyKeyboardMarkup([list(faq.keys())[i:i+2] for i in range(0, len(faq), 2)], resize_keyboard=True)

def start(update: Update, context: CallbackContext):
    update.message.reply_text("Привет 👋 Я бот фотостудии OH.STUDIO!\nВыбери вопрос из меню ниже 👇", reply_markup=main_keyboard)

def handle_message(update: Update, context: CallbackContext):
    user_text = update.message.text
    answer = faq.get(user_text)
    if answer:
        update.message.reply_text(answer)
    else:
        update.message.reply_text("Пожалуйста, выбери вопрос из списка ниже 👇", reply_markup=main_keyboard)

def main():
    token = os.getenv("BOT_TOKEN")
    if not token:
        print("❌ BOT_TOKEN не найден в переменных среды!")
        return

    updater = Updater(token)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(MessageHandler(Filters.text & ~Filters.command, handle_message))

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
