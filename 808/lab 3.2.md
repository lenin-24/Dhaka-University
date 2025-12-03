Here is a **clear, simple breakdown** of **exactly what you need to do** for the **Integrity Lab**, based fully on the PDF you uploaded.
(This is like the previous explanation but for the **Integrity** part.)

---

# ✅ **WHAT YOU NEED TO DO IN THE INTEGRITY LAB**

This lab teaches you how to **verify file integrity** using hashing.

You will perform tasks in **3 environments**:

1. **Windows CMD**
2. **Windows PowerShell**
3. **Kali Linux**

---

# 🔵 **PART 1 — Check File Integrity Using CMD.exe**

### ✔ 1. Check default hash (SHA1)

```
Certutil -hashfile "filename"
```

### ✔ 2. Check using a specific algorithm

```
Certutil -hashfile "filename" md5
```

📌 You can replace `md5` with:

* sha1
* sha256
* sha512
  etc.

---

# 🟢 **PART 2 — Check File Integrity Using PowerShell**

### ✔ 1. Get default hash (SHA256)

```
Get-FileHash "filename"
```

### ✔ 2. Specify hash algorithm

```
Get-FileHash "filename" -a SHA256
```

### ✔ 3. Compare file with a stored hash

You use this to check if a file has been changed or tampered.

```
(Get-FileHash "hash.txt" -a sha1).hash -eq "hash_of_the_file"
```

If output is:

* **True →** File matches the hash (no change)
* **False →** File does NOT match (file modified)

---

# 🔴 **PART 3 — Identify Hash Type Using Kali Linux (hash-identifier)**

### ✔ 1. Install hash-identifier (if missing)

```
sudo apt install hash-identifier
```

### ✔ 2. Run the program

```
hash-identifier
```

### ✔ 3. Paste any hash value

It will tell you what type the hash is (MD5, SHA1, SHA256, etc.)

---

# 🟡 **PART 4 — Hashing Files in Kali Linux**

### ✔ 1. MD5 Hash of a file

```
md5sum file.txt
```

### ✔ 2. MD5 Hash of a whole directory (recursive)

```
md5deep -r directory
```

md5deep:

* Calculates MD5 for every file inside folder + subfolders
* Useful for forensics and verifying many files at once

---

# ✅ **SUMMARY — EXACT LAB TASKS YOU MUST DO**

| Environment    | Task                        | Commands                                             |
| -------------- | --------------------------- | ---------------------------------------------------- |
| **CMD.exe**    | View SHA1 hash              | `Certutil -hashfile "file"`                          |
|                | View MD5/SHA256/SHA512 hash | `Certutil -hashfile "file" md5`                      |
| **PowerShell** | Default SHA256 hash         | `Get-FileHash "file"`                                |
|                | Specific hash               | `Get-FileHash "file" -a SHA256`                      |
|                | Compare hash                | `(Get-FileHash "hash.txt" -a sha1).hash -eq "value"` |
| **Kali Linux** | Identify hash               | `hash-identifier`                                    |
|                | MD5 file hash               | `md5sum file.txt`                                    |
|                | MD5 directory recursive     | `md5deep -r directory`                               |

---

If you want, I can also make:

✅ A **checklist** to follow during lab
✅ A **one-page cheat sheet** with all commands
✅ A **short explanation of each hashing algorithm**

Just tell me!
