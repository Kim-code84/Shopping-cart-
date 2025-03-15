# Shopping-cart-
from abc import ABC, abstractmethod


# Base Product class
class Product:
    def __init__(self, product_id, name, price, quantity):
        self.product_id = product_id
        self.name = name
        self.price = price
        self.quantity = quantity

    def update_quantity(self, new_quantity):
        self.quantity = new_quantity

    def get_product_info(self):
        return f"ID: {self.product_id}, Name: {self.name}, Price: ${self.price}, Quantity: {self.quantity}"


# Digital Product class
class DigitalProduct(Product):
    def __init__(self, product_id, name, price, quantity, file_size, download_link):
        super().__init__(product_id, name, price, quantity)
        self.file_size = file_size
        self.download_link = download_link

    def get_product_info(self):
        return super().get_product_info() + f", File Size: {self.file_size}MB, Download: {self.download_link}"


# Physical Product class
class PhysicalProduct(Product):
    def __init__(self, product_id, name, price, quantity, weight, dimensions, shipping_cost):
        super().__init__(product_id, name, price, quantity)
        self.weight = weight
        self.dimensions = dimensions
        self.shipping_cost = shipping_cost

    def get_product_info(self):
        return super().get_product_info() + f", Weight: {self.weight}kg, Dimensions: {self.dimensions}, Shipping Cost: ${self.shipping_cost}"


# Cart class
class Cart:
    def __init__(self):
        self.__cart_items = []

    def add_product(self, product):
        self.__cart_items.append(product)

    def remove_product(self, product_id):
        self.__cart_items = [p for p in self.__cart_items if p.product_id != product_id]

    def view_cart(self):
        return [p.get_product_info() for p in self.__cart_items]

    def calculate_total(self):
        return sum(p.price * p.quantity for p in self.__cart_items)


# User class
class User:
    def __init__(self, user_id, name):
        self.user_id = user_id
        self.name = name
        self.cart = Cart()

    def add_to_cart(self, product):
        self.cart.add_product(product)

    def remove_from_cart(self, product_id):
        self.cart.remove_product(product_id)

    def checkout(self, discount=None):
        total = self.cart.calculate_total()
        if discount:
            total = discount.apply_discount(total)
        self.cart = Cart()  # Clear cart after checkout
        return total


# Abstract Discount class
class Discount(ABC):
    @abstractmethod
    def apply_discount(self, total_amount):
        pass


# Percentage Discount class
class PercentageDiscount(Discount):
    def __init__(self, percentage):
        self.percentage = percentage

    def apply_discount(self, total_amount):
        return total_amount * (1 - self.percentage / 100)


# Fixed Amount Discount class
class FixedAmountDiscount(Discount):
    def __init__(self, amount):
        self.amount = amount

    def apply_discount(self, total_amount):
        return max(0, total_amount - self.amount)


# Testing
if __name__ == "__main__":
    # Create products
    digital1 = DigitalProduct(1, "E-Book", 10, 2, 5, "ebook.com/download")
    digital2 = DigitalProduct(2, "Online Course", 50, 1, 500, "course.com/access")
    physical1 = PhysicalProduct(3, "Laptop", 1000, 1, 2.5, "30x20x5cm", 20)
    physical2 = PhysicalProduct(4, "Phone", 500, 1, 0.5, "15x7x1cm", 10)
    physical3 = PhysicalProduct(5, "Headphones", 100, 1, 0.3, "10x10x5cm", 5)

    # Create users
    user1 = User(101, "Alice")
    user2 = User(102, "Bob")

    # Add products to users' carts
    user1.add_to_cart(digital1)
    user1.add_to_cart(digital2)
    user2.add_to_cart(physical1)
    user2.add_to_cart(physical2)
    user2.add_to_cart(physical3)

    # View carts
    print("User1 Cart:", user1.cart.view_cart())
    print("User2 Cart:", user2.cart.view_cart())

    # Apply discounts
    discount1 = PercentageDiscount(10)  # 10% discount
    discount2 = FixedAmountDiscount(50)  # $50 discount

    # Checkout
    total1 = user1.checkout(discount1)
    total2 = user2.checkout(discount2)

    print("User1 Total after Discount:", total1)
    print("User2 Total after Discount:", total2)
    from abc import ABC, abstractmethod


    # Product Class (Base Class)
    class Product:
        def __init__(self, product_id, name, price, quantity):
            self.product_id = product_id
            self.name = name
            self.price = price
            self.quantity = quantity

        def update_quantity(self, new_quantity):
            self.quantity = new_quantity

        def get_product_info(self):
            return f"ID: {self.product_id}, Name: {self.name}, Price: {self.price}, Quantity: {self.quantity}"


    # DigitalProduct Class (Derived from Product)
    class DigitalProduct(Product):
        def __init__(self, product_id, name, price, quantity, file_size, download_link):
            super().__init__(product_id, name, price, quantity)
            self.file_size = file_size
            self.download_link = download_link

        def get_product_info(self):
            return super().get_product_info() + f", File Size: {self.file_size}, Download: {self.download_link}"


    # PhysicalProduct Class (Derived from Product)
    class PhysicalProduct(Product):
        def __init__(self, product_id, name, price, quantity, weight, dimensions, shipping_cost):
            super().__init__(product_id, name, price, quantity)
            self.weight = weight
            self.dimensions = dimensions
            self.shipping_cost = shipping_cost

        def get_product_info(self):
            return super().get_product_info() + f", Weight: {self.weight}, Dimensions: {self.dimensions}, Shipping Cost: {self.shipping_cost}"


    # Cart Class
    class Cart:
        def __init__(self):
            self.__cart_items = []

        def add_product(self, product):
            self.__cart_items.append(product)

        def remove_product(self, product_id):
            self.__cart_items = [p for p in self.__cart_items if p.product_id != product_id]

        def view_cart(self):
            return [product.get_product_info() for product in self.__cart_items]

        def calculate_total(self):
            return sum(product.price * product.quantity for product in self.__cart_items)


    # User Class
    class User:
        def __init__(self, user_id, name):
            self.user_id = user_id
            self.name = name
            self.cart = Cart()

        def add_to_cart(self, product):
            self.cart.add_product(product)

        def remove_from_cart(self, product_id):
            self.cart.remove_product(product_id)

        def checkout(self, discount=None):
            total = self.cart.calculate_total()
            if discount:
                total = discount.apply_discount(total)
            self.cart = Cart()  # Clear cart after checkout
            return total


    # Abstract Discount Class
    class Discount(ABC):
        @abstractmethod
        def apply_discount(self, total_amount):
            pass


    # PercentageDiscount Class (Derived from Discount)
    class PercentageDiscount(Discount):
        def __init__(self, percentage):
            self.percentage = percentage

        def apply_discount(self, total_amount):
            return total_amount * (1 - self.percentage / 100)


    # FixedAmountDiscount Class (Derived from Discount)
    class FixedAmountDiscount(Discount):
        def __init__(self, amount):
            self.amount = amount

        def apply_discount(self, total_amount):
            return max(0, total_amount - self.amount)
import unittest

class TestShoppingCart(unittest.TestCase):
    def setUp(self):
        self.digital1 = DigitalProduct(1, "Ebook Python", 10, 1, "5MB", "https://download.ebook.com/python")
        self.physical1 = PhysicalProduct(3, "Laptop", 1000, 1, "2kg", "30x20x5cm", 20)
        self.user = User(1, "Alice")

    def test_add_product_to_cart(self):
        self.user.add_to_cart(self.digital1)
        self.assertEqual(len(self.user.cart.view_cart()), 1)

    def test_remove_product_from_cart(self):
        self.user.add_to_cart(self.physical1)
        self.user.remove_from_cart(3)
        self.assertEqual(len(self.user.cart.view_cart()), 0)

    def test_checkout_with_discount(self):
        self.user.add_to_cart(self.physical1)
        discount = PercentageDiscount(10)
        total = self.user.checkout(discount)
        self.assertEqual(total, 900)  # 1000 - 10%

if __name__ == "__main__":
    unittest.main()

 
