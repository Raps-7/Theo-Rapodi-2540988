# Theo-Rapodi-2540988
readme file for bookstore system
#THEO QUINTON RAPODI   2540988
import datetime


# --- PART 6: Object-Oriented Programming (OOP) ---
class Book:
    def __init__(self, title, price, stock):
        self.title = title
        self.price = price
        self.stock = stock

    def __str__(self):
        # --- PART 2: String Methods ---
        return f"{self.title.ljust(20)} | Price: {str(self.price).rjust(3)} BWP | Stock: {self.stock}"


# --- PART 4: Functions & PART 9: Program Functionality ---
def display_inventory(inventory):
    print("\n" + "=" * 30)
    print("      RAPS BOOKS INVENTORY")
    print("=" * 30)
    for i, book in enumerate(inventory, 1):
        print(f"{i}. {book}")


def process_purchase(inventory):
    display_inventory(inventory)
    try:
        # --- PART 1 & 5: Input & Conditional Logic ---
        cust_name = input("\nEnter Customer Name: ").strip().title()
        choice = int(input("Enter the number of the book to buy (0 to cancel): "))
        if choice == 0: return

        selected_book = inventory[choice - 1]
        qty = int(input(f"How many copies of '{selected_book.title}'? "))

        if 0 < qty <= selected_book.stock:
            # --- PART 3: Mathematical Operations & Discounts ---
            subtotal = qty * selected_book.price
            discount = 0

            # Applying 5% discount if spend is > 300 BWP
            if subtotal > 300:
                discount = subtotal * 0.05

            final_total = subtotal - discount
            selected_book.stock -= qty

            # --- PART 7 & 8: Modules & File I/O ---
            timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
            record = (f"{timestamp} | Cust: {cust_name} | Book: {selected_book.title} | "
                      f"Qty: {qty} | Total: {final_total:.2f} BWP (Disc: {discount:.2f})\n")

            with open("sales_records.txt", "a") as file:
                file.write(record)

            print(f"\n--- Receipt for {cust_name} ---")
            print(f"Subtotal: {subtotal} BWP")
            if discount > 0:
                print(f"Discount (5%): -{discount:.2f} BWP")
            print(f"FINAL TOTAL: {final_total:.2f} BWP")
        else:
            print("\nError: Invalid quantity or out of stock!")
    except (ValueError, IndexError):
        print("\nInvalid input. Please try again.")


def view_sales():
    print("\n--- RAPS BOOKS SALES HISTORY ---")
    try:
        with open("sales_records.txt", "r") as file:
            print(file.read())
    except FileNotFoundError:
        print("No transactions found yet.")


# --- PART 1: Main Structure ---
def main():
    # Initializing inventory
    inventory = [
        Book("Harry Potter", 200, 5),
        Book("Euphoria", 150, 3),
        Book("Spiderman Comic", 250, 7),
        Book("Cook Book", 120, 2),
        Book("Wednesday", 100, 5)
    ]

    while True:
        print("\n********************************")
        print("      WELCOME TO RAPS BOOKS     ")
        print("********************************")
        print("1. View Books\n2. New Sale\n3. Sales History\n4. Exit")
        choice = input("Select an option: ")

        if choice == '1':
            display_inventory(inventory)
        elif choice == '2':
            process_purchase(inventory)
        elif choice == '3':
            view_sales()
        elif choice == '4':
            print("Closing system. Have a great day!")
            break
        else:
            print("Invalid selection.")


if __name__ == "__main__":
    main()
