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
