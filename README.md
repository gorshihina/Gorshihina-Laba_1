# Gorshihina-Laba_1

Код:
import pandas as pd
# ШАГ 1: Загрузка и первичный осмотр данных
print("=" * 50)
print("ШАГ 1: Загрузка и первичный осмотр данных")
print("=" * 50)
# 1. Загрузим данные
try:
df = pd.read_csv('ncr_ride_bookings.csv')
print("Файл 'ncr_ride_bookings.csv' загружен успешно!")
except FileNotFoundError:
try:
df = pd.read_csv('датаcer_uber_pides_bookings.csv')
print("Файл 'датаcer_uber_pides_bookings.csv' загружен успешно!")
except FileNotFoundError:
print("ОШИБКА: Файл не найден!")
exit()
# 2. Выведем первые 5 строк
print("\n1. Первые 5 строк данных:")
print(df.head())
print("\n" + "-"*30)
# 3. Выведем общую информацию
print("2. Общая информация о данных:")
df.info()
print("\n" + "-"*30)
# 4. Выведем статистическое описание
print("3. Статистическое описание числовых столбцов:")
print(df.describe())
print("\n" + "-"*30)
# 5. Определим количество строк и столбцов
print("4. Количество строк и столбцов:")
print(f"Строк: {df.shape[0]}, Столбцов: {df.shape[1]}")
print("\n" + "-"*30)
# Покажем все столбцы для понимания структуры данных
print("ВСЕ СТОЛБЦЫ В ДАННЫХ:")
for i, col in enumerate(df.columns, 1):
print(f"{i}. {col}")
print("\n" + "-"*30)
# ШАГ 3: Выборка и фильтрация данных
print("=" * 50)
print("ШАГ 3: Выборка и фильтрация данных")
print("=" * 50)
# Посмотрим на данные чтобы понять какие столбцы использовать
print("Для выполнения заданий нам нужно определить правильные названия столбцов.")
print("Посмотрите на список выше и введите нужные названия:")
# Автоматически попробуем найти похожие столбцы
all_columns = df.columns.tolist()
# Попробуем найти столбцы по ключевым словам
booking_id_cols = [col for col in all_columns if 'booking' in col.lower() and 'id' in col.lower()]
datetime_cols = [col for col in all_columns if any(x in col.lower() for x in ['date', 'time', 'datetime'])]
status_cols = [col for col in all_columns if 'status' in col.lower()]
vehicle_cols = [col for col in all_columns if 'vehicle' in col.lower() or 'auto' in col.lower()]
payment_cols = [col for col in all_columns if 'payment' in col.lower()]
value_cols = [col for col in all_columns if 'value' in col.lower() or 'price' in col.lower() or 'cost' in col.lower()]
print(f"Возможные столбцы для Booking ID: {booking_id_cols}")
print(f"Возможные столбцы для даты/времени: {datetime_cols}")
print(f"Возможные столбцы для статуса: {status_cols}")
print(f"Возможные столбцы для типа транспорта: {vehicle_cols}")
print(f"Возможные столбцы для оплаты: {payment_cols}")
print(f"Возможные столбцы для стоимости: {value_cols}")

# Используем первые найденные столбцы или просим пользователя ввести
booking_id_col = booking_id_cols[0] if booking_id_cols else 'Booking ID'
datetime_col = datetime_cols[0] if datetime_cols else 'booking_datetime'
status_col = status_cols[0] if status_cols else 'Booking Status'
vehicle_col = vehicle_cols[0] if vehicle_cols else 'Vehicle Type'
payment_col = payment_cols[0] if payment_cols else 'Payment Method'
value_col = value_cols[0] if value_cols else 'Booking Value'
print(f"\nИспользуем столбцы:")
print(f"Booking ID: {booking_id_col}")
print(f"Дата/время: {datetime_col}")
print(f"Статус: {status_col}")
print(f"Тип транспорта: {vehicle_col}")
print(f"Оплата: {payment_col}")
print(f"Стоимость: {value_col}")
print("\n" + "-"*30)

# 1. Выберем только нужные столбцы
print("1. Выбранные столбцы (первые 5 строк):")
try:
selected_columns = df[[booking_id_col, datetime_col, status_col, vehicle_col, payment_col]]
print(selected_columns.head())
except KeyError as e:
print(f"Ошибка: столбец {e} не найден. Проверьте названия столбцов.")
print("\n" + "-"*30)
# 2. Фильтрация по статусу
print("2. Бронирования со статусом 'Cancelled by Driver':")
try:
cancelled_by_driver = df[df[status_col] == 'Cancelled by Driver']
print(cancelled_by_driver)
print(f"Найдено: {len(cancelled_by_driver)} записей")
except KeyError:
print("Столбец статуса не найден")
print("\n" + "-"*30)
# 3. Фильтрация по Auto и стоимости > 500
print("3. Бронирования Auto с Booking Value > 500:")
try:
auto_high_value = df[(df[vehicle_col] == 'Auto') & (df[value_col] > 500)]
print(auto_high_value)
print(f"Найдено: {len(auto_high_value)} записей")
except KeyError:
print("Не удалось выполнить фильтрацию. Проверьте названия столбцов.")
print("\n" + "-"*30)
# 4. Фильтрация по датам марта 2024
print("4. Бронирования за март 2024 года:")
try:
# Преобразуем дату в правильный формат
df[datetime_col] = pd.to_datetime(df[datetime_col])
march_2024 = df[(df[datetime_col] >= '2024-03-01') & (df[datetime_col] <= '2024-03-31')]
print(march_2024)
print(f"Найдено: {len(march_2024)} записей")
except KeyError:
print("Столбец с датой не найден")
except Exception as e:
print(f"Ошибка при работе с датами: {e}")
print("=" * 50)
print("АНАЛИЗ ЗАВЕРШЕН!")
print("=" * 50)

# Объяснение:
# 1. Импорт библиотеки
python
import pandas as pd
Что делает: Подключает библиотеку Pandas для работы с табличными данными. pd - это короткое имя для удобства.
# 2. Загрузка данных
<img width="639" height="239" alt="image" src="https://github.com/user-attachments/assets/a566abca-9969-497e-bc24-40af59240dbe" />
python
try:
df = pd.read_csv('ncr_ride_bookings.csv')
except FileNotFoundError:
try:
df = pd.read_csv('датаcer_uber_pides_bookings.csv')
except FileNotFoundError:
print("ОШИБКА: Файл не найден!")
exit()
Как работает: Пытается загрузить файл. Если первого файла нет - пробует второй. Если оба не найдены - останавливает программу.
# 3. Первичный анализ данных
python
print(df.head())      # Показывает первые 5 строк
df.info()            # Информация о данных (типы, количество)
print(df.describe()) # Статистика по числам
print(df.shape)      # Размер таблицы (строки, столбцы)
Зачем нужно: Чтобы понять что за данные, сколько их, какие типы информации.
# 4. Поиск нужных столбцов
python
booking_id_cols = [col for col in all_columns if 'booking' in col.lower() and 'id' in col.lower()]
Как работает: Ищет столбцы где в названии есть и 'booking' и 'id'. Например: "Booking_ID", "bookingId" и т.д.
# 5. Выборка столбцов
python
selected_columns = df[['Booking ID', 'booking_datetime', 'Booking Status', 'Vehicle Type', 'Payment Method']]
Что делает: Берет только указанные столбцы из всей таблицы.
# 6. Фильтрация данных
python
# Фильтр по статусу
cancelled_by_driver = df[df['Booking Status'] == 'Cancelled by Driver']
# Фильтр по двум условиям
auto_high_value = df[(df['Vehicle Type'] == 'Auto') & (df['Booking Value'] > 500)]
Как работает:
df[условие] - выбирает строки где условие True
& - логическое "И" (оба условия должны быть true)
== - проверка на равенство
# 7. Фильтрация по датам
python
df['booking_datetime'] = pd.to_datetime(df['booking_datetime'])  # Преобразует текст в дату
march_2024 = df[(df['booking_datetime'] >= '2024-03-01') & (df['booking_datetime'] <= '2024-03-31')]
Зачем: Чтобы можно было сравнивать даты как числа (больше/меньше).
Основные концепции:
DataFrame (df) - это таблица Excel в коде
Столбцы - как колонки в Excel
Строки - как строки в Excel
Фильтрация - выбор только нужных строк
Выборка - выбор только нужных столбцах
