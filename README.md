import telebot
from telebot import types
import random

# Токен вашего Telegram бота
TOKEN = 'YOUR_TELEGRAM_BOT_TOKEN'

# Инициализация бота
bot = telebot.TeleBot(TOKEN)

# Мотивационные цитаты
QUOTES = [
    "Каждый день дарует новую возможность стать лучше. — Марк Аврелий",
    "Мир принадлежит смелым. — Софокл",
    "Всё хорошее требует терпения. — Конфуций",
    "Начните с малого, мечтайте по-крупному. — Стив Джобс",
    "Движение — единственный путь к цели. — Михаил Булгаков",
    "Нет предела совершенству. — Лионель Месси",
    "Время бесценно, используйте его разумно. — Стивен Кови",
    "Только любовь движет миром. — Альберт Эйнштейн",
    "Делай то, что любишь, и успех придёт сам. — Уоррен Баффетт"
]

# Полезные советы
ADVICES = [
    "Улыбайтесь чаще!",
    "Отдыхайте регулярно.",
    "Занимайтесь любимым делом каждый день.",
    "Заботьтесь о своём здоровье и питании.",
    "Путешествуйте и узнавайте новое!"
]

# Случайные факты
FACTS = [
    "У осьминогов три сердца.",
    "Первое издание русской Библии было напечатано в 1491 году.",
    "Самый крупный остров в мире — Гренландия.",
    "Голуби умеют распознавать человеческие лица.",
    "Сон играет важную роль в процессе формирования долговременной памяти."
]

# Вопросы и варианты ответов для викторины
QUESTIONS = {
    "Какой самый быстрый наземный хищник?": ["Лев", "Гепард", "Ягуар"],
    "Как называется столица Франции?": ["Барселона", "Москва", "Париж"],
    "Какой континент самый жаркий?": ["Европа", "Африка", "Австралия"]
}
ANSWERS = {"Какой самый быстрый наземный хищник?": "Гепард", "Как называется столица Франции?": "Париж", "Какой континент самый жаркий?": "Африка"}

# Эмодзи и соответствующие реакции
EMOJI_REACTIONS = {
    "💥": "🎊 Тайное послание найдено!",
    "🔥": "🔥 Ваши идеи горят ярким пламенем!",
    "🎈": "🏁 Впереди победа, дерзайте!",
    "🍩": "😋 Хотите съесть шоколадку?",
    "🌶️": "⭐ Энергия притягивает удачу!",
    "🌴": "✨ Скоро лето, жара и приключения!",
    "📸": "👇 Покажите фотографии миру!",
    "🍰": "🍬 Выпечка порадует всех вокруг!",
    "🌹": "❄️ Цветы символизируют радость и любовь!",
    "🍵": "☕️ Теплый чай — идеальное завершение дня!",
    "🌿": "🌱 Рост начинается с маленького семени!",
    "🎯": "Цель достигнута, впереди новые высоты!",
    "🌲": "🌳 Прогуляйтесь по лесу, зарядитесь энергией природы!",
    "🍷": "🍺 Прекрасный повод расслабиться и отдохнуть!",
    "🌃": "Время для романтической прогулки под звездами!",
    "🍼": "Детство — самое беззаботное время, вспомните его!",
    "🌠": "Звездное небо полно тайн и магии!",
    "🍾": "За успешный день заслуживаете праздничного настроения!",
    "🍀": "Природа дарит вдохновение и красоту!",
    "🎮": "Победа в игре — это маленькая победа в жизни!",
    "🌍": "Мир огромен, исследуйте его с интересом!",
    "🌞": "Новый день приносит массу возможностей!",
    "🗽": "Мир полон красивых пейзажей, увидьте их!",
    "🍫": "Шоколад — сладкий символ счастья!",
    "🎰": "Верь в удачу, она тебя найдет!",
    "🌊": "Водные волны приглашают насладиться морским отдыхом!",
    "🚗": "Смелее двигайтесь вперед, дороги ведут к успеху!",
    "💜": "Любовь наполняет мир добром и радостью!",
    "🎬": "Кино — это искусство, позволяющее отвлечься и вдохновляться!",
    "🌬": "Свежий воздух придает жизненных сил и энергии!",
    "🌝": "Ночной мир полон загадочных звуков и ароматов!",
    "🌎": "Наша планета уникальна, давайте беречь её!"
}

# Переменная для хранения найденных пасхалок
USER_FOUND_EMOJIS = {}

# Приветственное сообщение
@bot.message_handler(commands=['start'])
def handle_start(message):
    USER_FOUND_EMOJIS.setdefault(message.chat.id, set())  # Обнуление статистики при каждом новом запуске
    bot.send_message(
        message.chat.id,
        f"Привет, {message.from_user.first_name}!\nЯ здесь, чтобы поддержать и вдохновить тебя.\nИспользуй команду /help, чтобы узнать мои возможности."
    )

# Меню возможностей
@bot.message_handler(commands=['help'])
def handle_help(message):
    keyboard = types.InlineKeyboardMarkup()
    button_quote = types.InlineKeyboardButton(text="Получить мотивирующую цитату", callback_data='quote')
    button_advice = types.InlineKeyboardButton(text="Получить полезный совет", callback_data='advice')
    button_fact = types.InlineKeyboardButton(text="Получить интересный факт", callback_data='fact')
    button_quiz = types.InlineKeyboardButton(text="Викторина", callback_data='quiz')
    keyboard.add(button_quote, button_advice, button_fact, button_quiz)
    bot.send_message(message.chat.id, "Что хочешь узнать?", reply_markup=keyboard)

# Обработка кликов по кнопкам
@bot.callback_query_handler(func=lambda call: True)
def handle_button_click(call):
    chat_id = call.message.chat.id
    
    if call.data == 'quote':
        quote = random.choice(QUOTES)
        bot.send_message(chat_id, quote)
        
    elif call.data == 'advice':
        advice = random.choice(ADVICES)
        bot.send_message(chat_id, advice)
        
    elif call.data == 'fact':
        fact = random.choice(FACTS)
        bot.send_message(chat_id, fact)
        
    elif call.data == 'quiz':
        question = random.choice(list(QUESTIONS.keys()))  # Выбор вопроса
        answers = QUESTIONS[question]  # Варианты ответов
        correct_answer = ANSWERS[question]  # Правильный ответ
        random.shuffle(answers)  # Перемешивание порядка ответов
        
        # Формирование inline-кнопок
        keyboard = types.InlineKeyboardMarkup()
        for ans in answers:
            btn = types.InlineKeyboardButton(ans, callback_data=f"answer_{ans}_{correct_answer}")
            keyboard.add(btn)
        
        bot.send_message(chat_id, question, reply_markup=keyboard)

# Викторина: проверка ответа
@bot.callback_query_handler(func=lambda call: call.data.startswith('answer'))
def quiz_check(call):
    _, given_answer, correct_answer = call.data.split('_')
    chat_id = call.message.chat.id
    if given_answer == correct_answer:
        bot.answer_callback_query(call.id, show_alert=True, text="Правильно! Молодец!")
    else:
        bot.answer_callback_query(call.id, show_alert=True, text="Неправильно :( Попробуй еще раз!")

# Реакция на эмодзи и подсчет найденных пасхалок
@bot.message_handler(func=lambda m: True)
def emoji_reaction(message):
    found_emojis = USER_FOUND_EMOJIS.get(message.chat.id, set())  # Загрузка текущего статуса
    total_emoji_count = len(EMOJI_REACTIONS)
    
    for emoji, reaction in EMOJI_REACTIONS.items():
        if emoji in message.text and emoji not in found_emojis:
            found_emojis.add(emoji)
            remaining_emojis = total_emoji_count - len(found_emojis)
            
            if remaining_emojis > 0:
                bot.send_message(message.chat.id, f"{reaction}\nМолодец! Осталось найти еще {remaining_emojis} пасхалки.")
            else:
                bot.send_message(message.chat.id, f"{reaction}\nПоздравляю! Ты нашел все пасхалки!")
                
            break  # Избежание множественной реакции на разные эмодзи в одном сообщении

# Запуск бота
print("Бот запущен")
bot.polling(non_stop=True)
