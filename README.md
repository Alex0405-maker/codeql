import hashlib
import datetime

class DigitalBillOfExchange:
    """
    Класс, представляющий цифровой вексель.
    """

    def __init__(self, issuer, payee, amount, due_date):
        """
        Инициализация векселя.
        :param issuer: Эмитент (выдавший вексель)
        :param payee: Получатель (кому выплачиваются средства)
        :param amount: Сумма векселя
        :param due_date: Дата погашения
        """
        self.issuer = issuer  # Эмитент
        self.payee = payee    # Получатель
        self.amount = amount  # Сумма
        self.due_date = due_date  # Дата погашения
        self.issue_date = datetime.datetime.now()  # Дата выдачи
        self.id = self.generate_id()  # Уникальный идентификатор векселя
        self.is_paid = False  # Флаг оплаты

    def generate_id(self):
        """
        Генерация уникального идентификатора для векселя.
        :return: Хэш как уникальный идентификатор
        """
        data = f"{self.issuer}{self.payee}{self.amount}{self.issue_date}"
        return hashlib.sha256(data.encode()).hexdigest()

    def pay(self):
        """
        Пометка векселя как оплаченного.
        """
        if self.is_paid:
            raise ValueError("Этот вексель уже был оплачен.")
        if datetime.datetime.now() > self.due_date:
            raise ValueError("Срок действия векселя истёк.")
        self.is_paid = True
        print(f"Вексель {self.id} оплачен.")

    def __str__(self):
        """
        Переопределение строкового представления объекта.
        :return: Строка с основными данными векселя
        """
        return (
            f"Цифровой вексель:\n"
            f"Эмитент: {self.issuer}\n"
            f"Получатель: {self.payee}\n"
            f"Сумма: {self.amount}\n"
            f"Дата выдачи: {self.issue_date}\n"
            f"Дата погашения: {self.due_date}\n"
            f"ID: {self.id}\n"
            f"Оплачен: {'Да' если self.is_paid else 'Нет'}"
        )


# Пример использования
if name == "__main__":
    # Создаём цифровой вексель
    issuer = "ООО 'Эмитент'"
    payee = "ООО 'Получатель'"
    amount = 100000  # Сумма в рублях
    due_date = datetime.datetime(2024, 12, 31)  # Дата погашения

    # Инициализация векселя
    bill = DigitalBillOfExchange(issuer, payee, amount, due_date)

    # Выводим информацию о векселе
    print(bill)

    # Помечаем вексель как оплаченный
    try:
        bill.pay()
    except ValueError as e:
        print(e)

    # Проверяем обновлённый статус
    print(bill)
