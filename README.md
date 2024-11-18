import telebot
from telebot.types import InlineKeyboardMarkup, InlineKeyboardButton

# Токен вашего бота, полученный от BotFather
TOKEN = 'YOUR_TELEGRAM_BOT_TOKEN_HERE'
bot = telebot.TeleBot(TOKEN)

# Словарь для хранения текстов, которые будут отображаться после нажатия на кнопку
info_texts = {
    'info_1': "Подготовка специалистов, профессионально занимающихся управлением, разбирающихся в широком спектре специальных вопросов стратегического и бизнес-планирования, производственного, информационного и финансового менеджмента, маркетинга, логистики, юриспруденции, психологии и многих других аспектов разработки и принятия управленческих решений.",
    'info_2': "Для поступления необходимо сдать следующие предметы: \n1. Русский язык \n2. Математика \nОдин из следующих предметов на выбор: \n• Обществознание \n• История \n• География \n• Иностранный язык",
    'info_3': "Вы выбрали Информацию 3! 🌟",
    'info_4': "Вы выбрали Информацию 4! 🌟",
    'info_5': "Вы выбрали Информацию 5! 🌟",
    'info_6': "Вы выбрали Информацию 6! 🌟",
    'info_7': "Вы выбрали Информацию 7! 🌟",
    'info_8': "Вы выбрали Информацию 8! 🌟",
    'info_9': "Вы выбрали Информацию 9! 🌟",
    'info_10': "Вы выбрали Информацию 10! 🌟",
}

# Обработчик команды /start
@bot.message_handler(commands=['start'])
def send_welcome(message):
    # Приветственное сообщение
    welcome_message = bot.reply_to(message, "Привет! 👋🏻 Какую информацию вы хотите получить? Нажмите на кнопку ниже.")

    # Создаем клавиатуру с кнопками
    keyboard = InlineKeyboardMarkup()

    # Создаем 10 кнопок
    button1 = InlineKeyboardButton("Расскажи о \nнаправлении", callback_data='info_1')
    button2 = InlineKeyboardButton("Какие предметы \nнужно сдать\n для поступления", callback_data='info_2')
    button3 = InlineKeyboardButton("Стоимость обучения", callback_data='info_3')
    button4 = InlineKeyboardButton("Какие дисциплины изучаются", callback_data='info_4')
    button5 = InlineKeyboardButton("Какие есть формы обучения", callback_data='info_5')
    button6 = InlineKeyboardButton("Трудоустройство", callback_data='info_6')
    button7 = InlineKeyboardButton("Стажировки", callback_data='info_7')
    button8 = InlineKeyboardButton("Должности", callback_data='info_8')
    button9 = InlineKeyboardButton("По какому адресу находится направление", callback_data='info_9')
    button10 = InlineKeyboardButton("Остались вопросы?", callback_data='info_10')

    # Объединяем кнопки в клавиатуре в столбик
    keyboard.add(button1)
    keyboard.add(button2)
    keyboard.add(button3)
    keyboard.add(button4)
    keyboard.add(button5)
    keyboard.add(button6)
    keyboard.add(button7)
    keyboard.add(button8)
    keyboard.add(button9)
    keyboard.add(button10)

    # Отправляем клавиатуру с кнопками
    bot.send_message(message.chat.id, "Выберите опцию:", reply_markup=keyboard)

# Обработчик нажатий кнопок
@bot.callback_query_handler(func=lambda call: True)
def handle_query(call):
    # Определяем ответ в зависимости от нажатой кнопки
    if call.data.startswith('info_'):
        handle_info_query(call)
    elif call.data in ['back_to_menu', 'request_another_info']:
        handle_secondary_action(call)

def handle_info_query(call):
    info_number = call.data
    response_text = info_texts.get(info_number, "Неизвестная информация")

    # Варианты действий после получения информации
    keyboard = InlineKeyboardMarkup()
    back_to_menu_button = InlineKeyboardButton("Вернуться в главное меню", callback_data='back_to_menu')
    request_another_info_button = InlineKeyboardButton("Запросить другую информацию", callback_data='request_another_info')

    keyboard.add(back_to_menu_button)
    keyboard.add(request_another_info_button)

    # Обновляем предыдущее сообщение, добавляя новые кнопки
    bot.edit_message_text(chat_id=call.message.chat.id, message_id=call.message.message_id,
                          text=response_text,
                          reply_markup=keyboard)

def handle_secondary_action(call):
    if call.data == 'back_to_menu':
        send_welcome(call.message)
    elif call.data == 'request_another_info':
        bot.edit_message_text(chat_id=call.message.chat.id, message_id=call.message.message_id,
                              text="Запрашиваем другую информацию. Пожалуйста, выберите:",
                              reply_markup=create_info_keyboard())

def create_info_keyboard():
    # Создаем клавиатуру для выбора информации
    keyboard = InlineKeyboardMarkup()
    button1 = InlineKeyboardButton("Расскажи о направлении", callback_data='info_1')
    button2 = InlineKeyboardButton("Какие предметы нужно сдать для поступления", callback_data='info_2')
    button3 = InlineKeyboardButton("Стоимость обучения", callback_data='info_3')
    button4 = InlineKeyboardButton("Какие дисциплины изучаются", callback_data='info_4')
    button5 = InlineKeyboardButton("Какие есть формы обучения", callback_data='info_5')
    button6 = InlineKeyboardButton("Трудоустройство", callback_data='info_6')
    button7 = InlineKeyboardButton("Стажировки", callback_data='info_7')
    button8 = InlineKeyboardButton("Должности", callback_data='info_8')
    button9 = InlineKeyboardButton("По какому адресу находится направление", callback_data='info_9')
    button10 = InlineKeyboardButton("Остались вопросы?", callback_data='info_10')

    keyboard.add(button1)
    keyboard.add(button2)
    keyboard.add(button3)
    keyboard.add(button4)
    keyboard.add(button5)
    keyboard.add(button6)
    keyboard.add(button7)
    keyboard.add(button8)
    keyboard.add(button9)
    keyboard.add(button10)

    return keyboard

# Запуск бота
if __name__ == '__main__':
    print("Бот запущен!")
    bot.polling(none_stop=True)
