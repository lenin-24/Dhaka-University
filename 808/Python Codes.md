## Assigning int ,string and float 


```python
x = 9
print(x)
print(type(x))

x = "abc"
print(x)
print(type(x))

x = 45.12
print(x)
print(type(x))
```

## Assigning multiple variables in same line
```python
x, y, z = 9, 5, "abc"
```

## python data types


### bool
```python
x = True
```
### list
```
x = [1,2,3,4,5.7, "abc"]
print(x)
print(x[4])
x[2] = 11
print(x)
x.append(2)
print(x)
print(len(x))
print(type(x))
```

### tuple
```
x = (1,2, "abc")
print(x)
print(x[0])
```
### dict
```
x  = {"bob": 10, "alice": 11, "john": 12}
print(x)
print(x["john"])
print(type(x))
```
## arithmetic operations
```
z = x % y # modulus
z = x // y # floor
z = x ** y # exponentiation
z = x/(y + y) - z # multiple operations
```
## casting
```
s = "123"
print(s)
print(type(s))
x = 1
print(x)
print(type(x))
y = int(s)      # str to int
print(y)
print(type(y))
z = x + y
print(z)

x = 11234.2345345
print(type(x))
y = int(x)      # float to int
print(y)
print(type(y))

s = "123.455"
print(type(s))
x = float(s)    # str to float
print(x)
print(type(x))
print(x +4)

s = "3"
x = float(s)    # str to float
print(x)
print(type(x))

y = 3
x = float(y)    # int to float
print(x)
print(type(x))

y = 3
s = str(y)      # int to str
print(s)
print(type(s))
```

# String Operations

```python
# String operations

s = "abc123"
print(s)
print(type(s))

print(len(s))  # length of a string

print(s[0])  # accessing characters by index
print(s[5])  # accessing characters by index

print(s[0:3])  # accessing multiple characters by index
print(s[2:4])  # accessing multiple characters by index
print(s[1:5])  # accessing multiple characters by index
print(s[2:])   # accessing multiple characters by index

s1 = "abc"
s2 = "xyz"
s3 = s1 + s2  # concat str with str
print(s3)

s = "abc"
x = 123
s1 = s + str(x)  # concat str with int
print(s1)

s = "abc"
x = 123.56
s1 = s + str(x)  # concat str with float
print(s1)

s = "AbC"
print(s.lower())  # to lowercase

s = "AbCde"
print(s.upper())      # to uppercase
print(s.swapcase())   # swapcase

s = "john"
print(s.capitalize())  # to uppercase first character

s = "abcdefghijk"
k1 = "def"
k2 = "ytg"
print(k1 in s)  # search in a string
print(k2 in s)  # search in a string

s = "           abc                xyz                  "
print(s.strip())  # strip

s = "abcd1234cd56"
s1 = s.replace("cd", "xyz")  # replacing a substring
print(s1)

s1 = s.replace("cd", "")     # removing a substring
print(s1)

s1 = s.replace("cd", " ")    # replacing a substring
print(s1)

s = "abc,def,xyz"
l = s.split(",")  # split
print(l)

s = "abc def xyz"
l = s.split(" ")  # split
print(l)

s = "abc, def, xyz"
l = s.split(", ")  # split
print(l)

s = "abcdefcdxyz"
l = s.split("cd")  # split
print(l)

s = "Topic sentences are similar to mini thesis statements. Like a thesis statement, a topic sentence has a specific main point. Like the thesis statement, a topic sentence has a unifying function."
l = s.split(". ")  # split
print(l)

s = "abc1234"
print(s.startswith("abc"))  # starts with
print(s.startswith("bc"))   # starts with
print(s.endswith("34"))     # ends with
print(s.endswith("bc"))     # ends with

s = "abcd1234cd"
k = "cd"
print(s.count(k))              # counting substring
print(s.count("9023902"))      # counting substring
print(s.count(k, 0, 5))        # count in slice [0:5]

s = "abxcd123"
k = "cd"
print(s.index(k))              # find index

s = "abxcd123cd23"
k = "cd"
print(s.index(k))              # find index
print(s.rindex(k))             # find right index

s1 = "1234"
s2 = "x1yz"
print(s1.isdigit())
print(s2.isdigit())
```

### If Statement

```
# if else
x = 5
y = 2
z = 3

if x > y:
  print("hello")
elif y > z:
  print("hello2")
elif x < z:
  print("hello3")
else:
  print("hello4")

# while loop with break
i = 1

while i <= 20:
  print(i)
  i += 1
  if i == 5:
    break

# while loop with continue

i = 0

while i <= 10:
  i += 1
  if i == 5:
    continue
  print(i)

# range function
print(list(range(10)))
print(list(range(0,10)))
```

### Loop
```
# for loop
for i in range(4):
  print(i)
print()

for i in range(4,10):
  print(i)
print()
```

### iterating a list
```
L = ["a", "b", "c"]

for i in L:
	print(i)
```
### iterating dictionary keys
```
d = {"a": 1, "b": 2, "c": 3}

for i in d:
	print(i)
```

### iterating dictionary keys and values
```
d = {"a": 1, "b": 2, "c": 3}

for i in d:
	print(i, d[i])
```


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
