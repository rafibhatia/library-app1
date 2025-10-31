# library-app1
a app to take out books from a library
import json
import uuid
import os

DATA_FILE = "library_data.json"

# -----------------------------
# Helpers for data management
# -----------------------------
def load_data():
    if not os.path.exists(DATA_FILE):
        return {"users": {}, "books": {}}
    with open(DATA_FILE, "r") as f:
        return json.load(f)

def save_data(data):
    with open(DATA_FILE, "w") as f:
        json.dump(data, f, indent=4)

# -----------------------------
# Core library functions
# -----------------------------
def register_user(name):
    data = load_data()
    user_id = str(uuid.uuid4())[:8]  # short unique ID
    data["users"][user_id] = {"name": name, "borrowed": []}
    save_data(data)
    print(f"✅ User '{name}' registered with ID: {user_id}")

def add_book(title):
    data = load_data()
    book_id = str(uuid.uuid4())[:8]
    data["books"][book_id] = {"title": title, "available": True, "borrowed_by": None}
    save_data(data)
    print(f"📚 Book '{title}' added with ID: {book_id}")

def borrow_book(user_id, book_id):
    data = load_data()
    if user_id not in data["users"]:
        print("❌ Invalid user ID.")
        return
    if book_id not in data["books"]:
        print("❌ Invalid book ID.")
        return

    book = data["books"][book_id]
    if not book["available"]:
        print("❌ Book already borrowed.")
        return

    book["available"] = False
    book["borrowed_by"] = user_id
    data["users"][user_id]["borrowed"].append(book_id)
    save_data(data)
    print(f"📖 '{book['title']}' borrowed successfully by {data['users'][user_id]['name']}!")

def return_book(user_id, book_id):
    data = load_data()
    if user_id not in data["users"] or book_id not in data["books"]:
        pri
