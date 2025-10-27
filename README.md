import json
import os

# === 1. Початкові дані ===
data_file = "employees.json"
result_file = "result.json"

# Якщо файл ще не існує — створюємо з початковими даними
if not os.path.exists(data_file):
    employees = [
        {"surname": "Іваненко", "address": "Київ, вул. Хрещатик 10", "position": "менеджер"},
        {"surname": "Петренко", "address": "Львів, вул. Галицька 23", "position": "програміст"},
        {"surname": "Сидоренко", "address": "Одеса, вул. Дерибасівська 5", "position": "аналітик"},
        {"surname": "Мельник", "address": "Харків, вул. Наукова 12", "position": "інженер"},
        {"surname": "Коваль", "address": "Дніпро, вул. Поля 3", "position": "бухгалтер"},
        {"surname": "Бондар", "address": "Полтава, вул. Шевченка 9", "position": "секретар"},
        {"surname": "Шевченко", "address": "Черкаси, вул. Героїв 15", "position": "маркетолог"},
        {"surname": "Ткаченко", "address": "Суми, вул. Соборна 7", "position": "менеджер"},
        {"surname": "Олійник", "address": "Рівне, вул. Миру 4", "position": "програміст"},
        {"surname": "Кравчук", "address": "Житомир, вул. Перемоги 1", "position": "дизайнер"}
    ]
    with open(data_file, "w", encoding="utf-8") as f:
        json.dump(employees, f, ensure_ascii=False, indent=4)

# === 2. Функції роботи з JSON ===

def load_data():
    with open(data_file, "r", encoding="utf-8") as f:
        return json.load(f)

def save_data(data):
    with open(data_file, "w", encoding="utf-8") as f:
        json.dump(data, f, ensure_ascii=False, indent=4)

def show_all():
    data = load_data()
    print("\n--- Вміст JSON файлу ---")
    for emp in data:
        print(f"{emp['surname']} — {emp['address']} — {emp['position']}")
    print()

def add_employee():
    data = load_data()
    surname = input("Введіть прізвище: ")
    address = input("Введіть адресу: ")
    position = input("Введіть посаду: ")
    data.append({"surname": surname, "address": address, "position": position})
    save_data(data)
    print("✓ Запис додано.\n")

def delete_employee():
    data = load_data()
    surname = input("Введіть прізвище для видалення: ")
    new_data = [emp for emp in data if emp["surname"].lower() != surname.lower()]
    if len(new_data) == len(data):
        print("! Такого працівника не знайдено.")
    else:
        save_data(new_data)
        print("✓ Запис видалено.\n")

def search_employee():
    data = load_data()
    key = input("Виберіть поле для пошуку (surname/address/position): ").strip().lower()
    value = input("Введіть значення для пошуку: ").strip().lower()
    results = [emp for emp in data if value in emp[key].lower()]
    if results:
        print("\nЗнайдено:")
        for emp in results:
            print(f"{emp['surname']} — {emp['address']} — {emp['position']}")
    else:
        print("Нічого не знайдено.\n")

def find_by_letter():
    """Завдання за варіантом"""
    data = load_data()
    letter = input("Введіть першу літеру прізвища: ").strip().lower()
    results = [emp for emp in data if emp["surname"].lower().startswith(letter)]

    if results:
        print(f"\n ✓ Працівники, чиї прізвища починаються на '{letter.upper()}':")
        for emp in results:
            print(f"{emp['surname']} — {emp['address']}")
        with open(result_file, "w", encoding="utf-8") as f:
            json.dump(results, f, ensure_ascii=False, indent=4)
        print(f"\nРезультати записано у файл {result_file}")
    else:
        print("Немає співробітників із такою літерою.")

# === 3. Головне меню ===
def main():
    while True:
        print("\n--- МЕНЮ ---")
        print("1. Показати всі записи")
        print("2. Додати працівника")
        print("3. Видалити працівника")
        print("4. Пошук за полем")
        print("5. Завдання (пошук за літерою прізвища)")
        print("0. Вихід")

        choice = input("Ваш вибір: ")
        if choice == "1":
            show_all()
        elif choice == "2":
            add_employee()
        elif choice == "3":
            delete_employee()
        elif choice == "4":
            search_employee()
        elif choice == "5":
            find_by_letter()
        elif choice == "0":
            print("Завершення роботи.")
            break
        else:
            print("Невірний вибір, спробуйте ще раз.")

# === Запуск програми ===
if __name__ == "__main__":
    main()
