# Symmetric Encryption with Fernet (cryptography)

Demonstrates how to encrypt and decrypt data using the `Fernet` symmetric encryption scheme from the `cryptography` library.

> 🔐 **Note**: The key must be kept secret and stored securely. Never hardcode it in production code.

```python
from cryptography.fernet import Fernet

# 1. Generate a key (do this once and store it securely)
key = Fernet.generate_key()
print(f"Encryption Key: {key.decode()}")

# 2. Create cipher
cipher = Fernet(key)

# 3. Encrypt a message
plaintext = "SensitiveData123"
ciphertext = cipher.encrypt(plaintext.encode())
print(f"Encrypted: {ciphertext.decode()}")

# 4. Decrypt the message
decrypted = cipher.decrypt(ciphertext).decode()
print(f"Decrypted: {decrypted}")
```

## Expected Output (example):
```
Encryption Key: 5j6x8Kl2mN0pQrStUvWxYzAbCdEfGhIjKlMnOpQrStUvWxYz1234567890ABCD
Encrypted: gAAAAABk... (long base64 string)
Decrypted: SensitiveData123
```

> 💡 **Installation**: If you don't have the `cryptography` library, install it via:  
> ```bash
> pip install cryptography
> ```

# Password Hashing: With and Without Salt

Demonstrates the importance of salting passwords during hashing to prevent identical hashes for the same password across users.

> 🔒 **Best Practice**: Always use a **unique salt** per password. For production systems, consider using `bcrypt`, `scrypt`, or `argon2` instead of plain SHA-256.

```python
import hashlib
import os
import secrets

# Function to hash a password without salt
def hash_password_without_salt(password):
    return hashlib.sha256(password.encode()).hexdigest()

# Function to hash a password with salt
def hash_password_with_salt(password, salt=None):
    if not salt:
        salt = secrets.token_hex(16)  # Generate a 16-byte (32 hex char) salt
    salted_password = salt + password
    hashed = hashlib.sha256(salted_password.encode()).hexdigest()
    return salt, hashed

# Simulate users with the same password
password = "mypassword123"

print("=== Without Salt ===")
hash1 = hash_password_without_salt(password)
hash2 = hash_password_without_salt(password)
print(f"User1 Hash: {hash1}")
print(f"User2 Hash: {hash2}")
print("Are hashes equal?", hash1 == hash2)

print("\n=== With Salt ===")
salt1, hash1 = hash_password_with_salt(password)
salt2, hash2 = hash_password_with_salt(password)
print(f"User1 Salt: {salt1}")
print(f"User1 Hash: {hash1}")
print(f"User2 Salt: {salt2}")
print(f"User2 Hash: {hash2}")
print("Are hashes equal?", hash1 == hash2)
```

## Sample Output
```
=== Without Salt ===
User1 Hash: e0c67e5e8f9a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6
User2 Hash: e0c67e5e8f9a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6
Are hashes equal? True

=== With Salt ===
User1 Salt: a1b2c3d4e5f678901234567890abcdef
User1 Hash: 9f8e7d6c5b4a3210fedcba9876543210abcdef1234567890fedcba9876543210
User2 Salt: 0987654321fedcba0987654321abcdef
User2 Hash: 1a2b3c4d5e6f7890abcdef1234567890fedcba0987654321abcdef1234567890
Are hashes equal? False
```

> 💡 **Note**: While this example uses `secrets.token_hex` and SHA-256 for clarity, **real-world applications should use dedicated password hashing libraries** like `bcrypt`:
> ```python
> import bcrypt
> salt = bcrypt.gensalt()
> hashed = bcrypt.hashpw(password.encode(), salt)
> ```

# File Encryption with Fernet

Encrypts each line of a text file (`main.txt`) using symmetric encryption and saves the encrypted content to `encrypted.txt`. The encryption key is stored separately in `secret.key`.

> 🔐 **Security Note**: Keep `secret.key` secure and never share it. Anyone with this key can decrypt your data.

```python
from cryptography.fernet import Fernet

# 1. Generate a key and save it to a file
key = Fernet.generate_key()
with open("secret.key", "wb") as key_file:
    key_file.write(key)

# 2. Create cipher
cipher = Fernet(key)

# 3. Read from a text file and encrypt each line
with open("main.txt", "r") as f:
    lines = f.readlines()

# 4. Encrypt lines
encrypted_lines = [cipher.encrypt(line.strip().encode()).decode() for line in lines]

# 5. Save encrypted lines to output file
with open("encrypted.txt", "w") as f:
    for line in encrypted_lines:
        f.write(line + "\n")

print("Encryption complete. Encrypted lines saved to encrypted.txt and key saved to secret.key.")
```

## Usage Instructions
1. Create a file named `main.txt` in the same directory with your plaintext content (one entry per line).
2. Run the script.
3. Two new files will be created:
   - `secret.key` — your encryption key (keep this safe!)
   - `encrypted.txt` — encrypted version of `main.txt`

> 💡 **To decrypt later**, load the key from `secret.key` and use `cipher.decrypt()` on each line of `encrypted.txt`.

> 📦 **Install dependency** (if not already installed):
> ```bash
> pip install cryptography
> ```

# File Decryption with Fernet

Decrypts each line of an encrypted file (`encrypted.txt`) using the key stored in `secret.key`, and outputs the original plaintext.

> 🔑 **Requirement**: You must have the `secret.key` file generated during encryption. Without it, decryption is impossible.

```python
from cryptography.fernet import Fernet

# 1. Load the key
with open("secret.key", "rb") as key_file:
    key = key_file.read()

cipher = Fernet(key)

# 2. Read encrypted lines
with open("encrypted.txt", "r") as f:
    encrypted_lines = f.readlines()

# 3. Decrypt each line
decrypted_lines = [cipher.decrypt(line.strip().encode()).decode() for line in encrypted_lines]

# 4. Print or save
print("Decrypted content:")
for line in decrypted_lines:
    print(line)

# Optionally save to a file
with open("decrypted.txt", "w") as f:
    for line in decrypted_lines:
        f.write(line + "\n")
```

## Usage Instructions
1. Ensure the following files are in your working directory:
   - `secret.key` — the key generated during encryption
   - `encrypted.txt` — the file produced by the encryption script
2. Run the script.
3. The original content will be:
   - Printed to the console
   - Saved to `decrypted.txt` (exact copy of the original `main.txt`)

> 💡 **Note**: This script assumes each line in `encrypted.txt` was encrypted independently (as in the companion encryption script).

> 📦 **Install dependency** (if needed):
> ```bash
> pip install cryptography
> ```

# SQLite In-Memory Database Example

Demonstrates creating a table, inserting multiple records, and querying data using an in-memory SQLite database with Python’s `sqlite3` module.

> 💡 **Note**: An in-memory database (`:memory:`) exists only during the program’s execution and is **not persisted to disk**.

```python
import sqlite3

# Connect to an in-memory SQLite database
conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

# Create a table
cursor.execute('''
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    password TEXT NOT NULL
)
''')

# Insert data
users = [
    ('alice', 'password123'),
    ('bob', 'qwerty'),
    ('charlie', 'letmein')
]
cursor.executemany("INSERT INTO users (username, password) VALUES (?, ?)", users)

# Query and display data
cursor.execute("SELECT * FROM users")
for row in cursor.fetchall():
    print(row)

# Close the connection
conn.close()
```

## Expected Output
```
(1, 'alice', 'password123')
(2, 'bob', 'qwerty')
(3, 'charlie', 'letmein')
```

> ⚠️ **Security Reminder**: Storing plaintext passwords (as shown here for simplicity) is **unsafe**. In real applications, always store **salted password hashes** (e.g., using `bcrypt` or `argon2`).

> ✅ **No external dependencies**: The `sqlite3` module is part of Python’s standard library.

## Simulate a student login system. Your task is to: Create a table students with fields: id, name, roll, password. Insert at least 3 students.
# SQLite Example: Vulnerable Students Table (In-Memory)

Creates an in-memory SQLite database with a `students` table that stores plaintext passwords — illustrating a **common security anti-pattern**.

> ⚠️ **Warning**: Storing passwords in plaintext is **highly insecure**. This example is for educational purposes only.

```python
import sqlite3

# Create an in-memory SQLite database
conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

# Create a vulnerable students table
cursor.execute('''
CREATE TABLE students (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    roll TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL
)
''')

# Insert sample data
students = [
    ('Tanvir', '2023-001', 'topsecret'),
    ('Nadia', '2023-002', 'password1'),
    ('Imran', '2023-003', 'hunter2')
]
cursor.executemany("INSERT INTO students (name, roll, password) VALUES (?, ?, ?)", students)

cursor.execute("SELECT * FROM students")
for row in cursor.fetchall():
    print(row)

# Close the connection
conn.close()
```

## Expected Output
```
(1, 'Tanvir', '2023-001', 'topsecret')
(2, 'Nadia', '2023-002', 'password1')
(3, 'Imran', '2023-003', 'hunter2')
```

> ✅ **Best Practice**: In real applications:
> - **Never store plaintext passwords**
> - Use strong hashing (e.g., `bcrypt`, `argon2`) with unique salts
> - Consider using parameterized queries consistently (already done here ✅)
> - Validate and sanitize all inputs

> 💡 The `:memory:` database is temporary and vanishes when the connection closes — useful for testing, not for persistent storage.

## Create a new Student table with fields: id, name, roll, password, salt. Take name, roll, and password as input from the console. Then hash the password after adding salt, save the hashed password and salt.


# Secure Student Registration with Salted Password Hashing

Demonstrates how to securely store user passwords in an SQLite database using **unique random salts** and SHA-256 hashing.

> 🔒 **Security Practice**: Each password is hashed with a unique salt to prevent rainbow table attacks and ensure identical passwords produce different hashes.

```python
import sqlite3
import hashlib
import os

# Connect to an in-memory database (or use "students.db" for file-based)
conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

# Create the Student table
cursor.execute('''
CREATE TABLE Student (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    roll TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    salt TEXT NOT NULL
)
''')

# Function to hash a password with a salt
def hash_password(password, salt):
    return hashlib.sha256((password + salt).encode()).hexdigest()

# Insert 3 students
for i in range(3):
    print(f"\nEnter details for student #{i+1}")
    name = input("  Name: ").strip()
    roll = input("  Roll: ").strip()
    raw_password = input("  Password: ").strip()

    salt = os.urandom(16).hex()  # Generate a 16-byte random salt as hex
    hashed_password = hash_password(raw_password, salt)

    cursor.execute(
        "INSERT INTO Student (name, roll, password, salt) VALUES (?, ?, ?, ?)",
        (name, roll, hashed_password, salt)
    )

# Display the students in the database
print("\n✅ All students inserted into database:\n")
cursor.execute("SELECT id, name, roll, password, salt FROM Student")
students = cursor.fetchall()

for student in students:
    print(f"ID: {student[0]}")
    print(f"Name: {student[1]}")
    print(f"Roll: {student[2]}")
    print(f"Hashed Password: {student[3]}")
    print(f"Salt: {student[4]}")
    print("-" * 40)

conn.close()
```

## How It Works
1. **Unique Salt per User**: `os.urandom(16).hex()` generates a cryptographically secure 16-byte random salt.
2. **Salted Hashing**: Password + salt are concatenated and hashed with SHA-256.
3. **Secure Storage**: Both the hash **and** the salt are stored in the database.
4. **Verification Ready**: To verify a login, retrieve the salt, hash the input password with it, and compare to the stored hash.

## Sample Run (Interactive)
```
Enter details for student #1
  Name: Alice
  Roll: 2023-001
  Password: secret123

Enter details for student #2
  Name: Bob
  Roll: 2023-002
  Password: secret123

✅ All students inserted into database:

ID: 1
Name: Alice
Roll: 2023-001
Hashed Password: a1b2c3... (unique hash)
Salt: f8e7d6... (random salt)
----------------------------------------
ID: 2
Name: Bob
Roll: 2023-002
Hashed Password: x9y8z7... (different hash!)
Salt: a1b2c3... (different salt)
----------------------------------------
```

> 💡 **For Production**: While this is a significant improvement over plaintext, consider using **dedicated password hashing libraries** like `bcrypt`, `scrypt`, or `argon2` (via `passlib` or `argon2-cffi`), which are **slower by design** and more resistant to brute-force attacks.

> 📌 **Note**: This example uses an in-memory database (`:memory:`). To persist data, replace `":memory:"` with `"students.db"`.

## Take the roll and password of a student from input. Now if the ceredentials is correct, print sucessful login. Otherwise, print an error message. If possible, perform the login using a function.


# Secure Storage + Intentionally Vulnerable Login (SQL Injection Demo)

This script demonstrates:
1. ✅ **Secure password storage** using unique random salts and SHA-256 hashing.
2. ⚠️ **Intentionally vulnerable login logic** that is susceptible to **SQL injection**—for educational purposes only.

> 🛑 **Never use the `login()` function below in real applications!** It illustrates how **NOT** to handle user input.

```python
import sqlite3
import hashlib
import os

# Connect to an in-memory database (or use "students.db" for file-based)
conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

# Create the Student table
cursor.execute('''
CREATE TABLE Student (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    roll TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    salt TEXT NOT NULL
)
''')

# Function to hash a password with a salt
def hash_password(password, salt):
    return hashlib.sha256((password + salt).encode()).hexdigest()

# Insert 3 students
for i in range(3):
    print(f"\nEnter details for student #{i+1}")
    name = input("  Name: ").strip()
    roll = input("  Roll: ").strip()
    raw_password = input("  Password: ").strip()

    salt = os.urandom(16).hex()
    hashed_password = hash_password(raw_password, salt)

    cursor.execute(
        "INSERT INTO Student (name, roll, password, salt) VALUES (?, ?, ?, ?)",
        (name, roll, hashed_password, salt)
    )

# Display the students in the database
print("\n✅ All students inserted into database:\n")
cursor.execute("SELECT id, name, roll, password, salt FROM Student")
students = cursor.fetchall()

for student in students:
    print(f"ID: {student[0]}")
    print(f"Name: {student[1]}")
    print(f"Roll: {student[2]}")
    print(f"Hashed Password: {student[3]}")
    print(f"Salt: {student[4]}")
    print("-" * 40)


def login():
    """
    ⚠️  WARNING: This login function is intentionally vulnerable to SQL injection!
    This is for educational purposes only - NEVER use this in production code!
    """
    print("\n🔐 Student Login System")
    print("=" * 30)

    roll = input("Enter Roll Number: ").strip()
    password = input("Enter Password: ").strip()

    # VULNERABLE QUERY - Direct string concatenation without sanitization
    query = f"SELECT id, name, roll, salt FROM Student WHERE roll = '{roll}'"
    print(f"\n🔍 Executing query: {query}")

    try:
        cursor.executescript(query)  # ⚠️ Dangerous: allows multiple statements
        result = cursor.fetchone()

        if result:
            student_id, name, student_roll, salt = result

            # Get the stored password hash
            cursor.execute(f"SELECT password FROM Student WHERE id = {student_id}")
            stored_hash = cursor.fetchone()[0]

            # Verify password
            input_hash = hash_password(password, salt)

            if input_hash == stored_hash:
                print(f"\n✅ Login successful!")
                print(f"Welcome, {name} (Roll: {student_roll})")
                return True
            else:
                print("\n❌ Invalid password!")
                return False
        else:
            print("\n❌ Student not found!")
            return False

    except sqlite3.Error as e:
        print(f"\n💥 Database error: {e}")
        return False

# Run login twice to demonstrate behavior
login()
login()

conn.close()
```

## 🔥 SQL Injection Risk Example

The vulnerable line:
```python
query = f"SELECT ... WHERE roll = '{roll}'"
```
allows an attacker to input something like:
```
Roll Number: 2023-001' --
Password: anything
```
Or worse:
```
Roll Number: '; DROP TABLE Student; --
```
→ Which could **bypass authentication** or **destroy data**.

> 💡 **Secure Alternative**: Always use **parameterized queries**:
> ```python
> cursor.execute("SELECT ... WHERE roll = ?", (roll,))
> ```

## Key Takeaways
- ✅ Passwords are **securely hashed with unique salts**.
- ❌ User input is **directly interpolated into SQL** → **SQL injection**.
- 🚫 `cursor.executescript()` allows **multiple SQL statements** — extremely dangerous with user input.
- ✅ Use `cursor.execute()` with **parameter placeholders (`?`)** for safety.

> 📚 This script is ideal for **teaching secure coding principles** and **demonstrating attack vectors responsibly**.

> ⚠️ **Reminder**: Close database connections and avoid in-memory databases in production scenarios.
>

# SQL Injection Demo: Vulnerable vs. Safe Login

This script demonstrates how **SQL injection** can bypass authentication in a vulnerable login function, and how **parameterized queries** prevent it.

> 🛡️ **Key Lesson**: Always use **parameterized queries** (`?` placeholders) — never interpolate user input directly into SQL strings.

```python
import sqlite3

# Step 1: Setup in-memory database
conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

# Create users table
cursor.execute("""
CREATE TABLE users (
    username TEXT,
    password TEXT
)
""")

# Insert a sample user
cursor.execute("INSERT INTO users VALUES ('admin', 'password123')")
conn.commit()

# Step 2: Vulnerable login function
def vulnerable_login(username, password):
    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
    print("Executing Query:", query)
    cursor.execute(query)
    result = cursor.fetchone()
    return result is not None

# Step 3: SQL injection attempt
# Injection payload that always returns true: "' OR '1'='1"
print("\n--- Vulnerable Login ---")
username_input = "admin"
password_input = "' OR '1'='1"
if vulnerable_login(username_input, password_input):
    print("Logged in (VULNERABLE)")
else:
    print("Login failed")

# Step 4: Safe version using parameterized query
def safe_login(username, password):
    query = "SELECT * FROM users WHERE username = ? AND password = ?"
    print("Executing Query:", query)
    cursor.execute(query, (username, password))
    result = cursor.fetchone()
    return result is not None

print("\n--- Safe Login ---")
if safe_login(username_input, password_input):
    print("Logged in (SAFE)")
else:
    print("Login failed")
```

## Expected Output
```
--- Vulnerable Login ---
Executing Query: SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1'
Logged in (VULNERABLE)

--- Safe Login ---
Executing Query: SELECT * FROM users WHERE username = ? AND password = ?
Login failed
```

## 🔍 What Happened?

### ❌ Vulnerable Query (Dangerous!)
```sql
SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1'
```
- The condition `'1'='1'` is always **true**.
- Due to operator precedence, this becomes:  
  `(password = '') OR ('1'='1')` → **TRUE**, so the row is returned.
- **Authentication bypassed!**

### ✅ Safe Query (Secure!)
- The input `' OR '1'='1` is treated as a **literal string**, not SQL code.
- The database looks for a password **exactly equal** to that string — which doesn’t exist.
- **Injection neutralized.**

## ✅ Best Practices
- **Never** use f-strings or `%` formatting to build SQL queries with user input.
- **Always** use **parameterized queries** (`cursor.execute(query, (param1, param2))`).
- Validate and sanitize input as an extra layer (but **never rely on it alone**).

> 💡 **Note**: This example uses plaintext passwords for simplicity. In real apps, store **salted password hashes**, not raw passwords.

> 📦 No external dependencies — uses Python’s built-in `sqlite3`.
