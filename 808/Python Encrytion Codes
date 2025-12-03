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
