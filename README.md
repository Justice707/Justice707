- 👋 Hi, I’m @Justice707
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Justice707/Justice707 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
from flask import Flask, render_template, request, redirect, url_for
import sqlite3

app = Flask(__name__)

# SQLite database setup
conn = sqlite3.connect('store.db')
cursor = conn.cursor()
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT,
    email TEXT UNIQUE,
    address TEXT,
    phone_number TEXT,
    kyc_verified BOOLEAN DEFAULT False,
    score INTEGER DEFAULT 1,
    positive_feedback_percentage INTEGER DEFAULT 100
);

CREATE TABLE IF NOT EXISTS items (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER,
    name TEXT,
    description TEXT,
    price REAL,
    status TEXT DEFAULT 'Available',
    FOREIGN KEY(user_id) REFERENCES users(id)
);

CREATE TABLE IF NOT EXISTS transactions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    buyer_id INTEGER,
    seller_id INTEGER,
    item_id INTEGER,
    tracking_number TEXT,
    status TEXT DEFAULT 'Pending',
    FOREIGN KEY(buyer_id) REFERENCES users(id),
    FOREIGN KEY(seller_id) REFERENCES users(id),
    FOREIGN KEY(item_id) REFERENCES items(id)
);

CREATE TABLE IF NOT EXISTS reviews (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    reviewer_id INTEGER,
    reviewed_id INTEGER,
    transaction_id INTEGER,
    feedback TEXT,
    rating INTEGER,
    FOREIGN KEY(reviewer_id) REFERENCES users(id),
    FOREIGN KEY(reviewed_id) REFERENCES users(id),
    FOREIGN KEY(transaction_id) REFERENCES transactions(id)
);
''')
conn.commit()
conn.close()

# Flask routes and logic would go here...

if __name__ == '__main__':
    app.run(debug=True)
