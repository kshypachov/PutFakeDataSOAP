
# SOAP Клієнт для створення користувачів

Цей проект містить Python-клієнт, який взаємодіє з SOAP веб-сервісом для створення нових користувачів.

## Вимоги

Для запуску цього проекту вам потрібні наступні бібліотеки:

- `zeep` - SOAP клієнт для Python.
- `requests` - HTTP бібліотека для Python.
- `faker` - бібліотека для генерації фейкових даних.
- `Python 10+`

Ви можете встановити їх за допомогою наступної команди:

```bash
pip install zeep requests Faker
```

## Конфігурація

Переконайтеся, що ваш SOAP веб-сервіс запущений і доступний за URL `http://localhost:8000/?wsdl`. Якщо ваш сервіс знаходиться за іншою адресою, оновіть значення змінної `wsdl` у коді клієнта.

## Використання

1. Клонуйте репозиторій або завантажте код.
```bash
git clone https://github.com/kshypachov/soap_web_service_sync.git
```
2. Переконайтеся, що ваш SOAP веб-сервіс запущений.

3. Запустіть скрипт:

```bash
python client.py
```

Скрипт згенерує 10 фейкових користувачів та відправить їх до веб-сервісу для створення.

## Приклад коду

```python
from zeep import Client
from zeep.transports import Transport
from requests import Session
from faker import Faker

fake = Faker(['uk_UA'])
count = 10

# Вказуємо URL WSDL-файлу
wsdl = 'http://localhost:8000/?wsdl'

session = Session()
transport = Transport(session=session)
# Створюємо клієнта
client = Client(wsdl=wsdl, transport=transport)

for _ in range(count):

    firstname = fake.first_name_male()
    surname = fake.last_name_male()
    patronymic = fake.middle_name_male()
    dateOfBirth = fake.date()
    rnokpp = str(fake.random_number(digits=10, fix_len=True))
    unzr = dateOfBirth.replace("-","") + "-" + str(fake.random_number(digits=5, fix_len=True))
    pasportNumber = str(fake.random_number(digits=9, fix_len=True))
    sex = "male"
    lineitems = []

    # Створюємо дані для нового користувача
    person_data = {
        "name": firstname,
        "surname": surname,
        "patronym":  patronymic,
        "dateOfBirth": dateOfBirth,
        "gender": "male",
        "rnokpp": rnokpp,
        "passportNumber": pasportNumber,
        "unzr": unzr
    }

    # Викликаємо метод create_person
    response = client.service.create_person(person_data)

    # Друкуємо відповідь
    print(response)

print("The END!")
```