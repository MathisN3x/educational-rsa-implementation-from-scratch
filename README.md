# educational-rsa-implementation-from-scratch
# 🔐 Educational RSA Implementation From Scratch

This project demonstrates a fully manual and educational implementation of the RSA cryptosystem using Python. It walks through the entire process — from generating large prime numbers to encrypting and decrypting messages — without relying on external cryptography libraries.

> 📘 This implementation is for **educational purposes only** and not suitable for real-world security applications. It omits cryptographic padding and safety considerations required for secure deployments.

---

## 🧠 What You'll Learn

- The mathematics behind RSA: Euler’s theorem, modular inverses, prime generation.
- How to implement each step of RSA manually in Python.
- Encoding text into numbers and vice versa for cryptographic purposes.
- Common pitfalls and how to interpret results when something goes wrong.

---

## 📂 Files

| File               | Description                             |
|--------------------|-----------------------------------------|
| `RSA_test.ipynb`   | Main Jupyter notebook with code + demos |

---

## ⚙️ Features

✅ Manual RSA key generation  
✅ Encryption and decryption of ASCII-based messages  
✅ Prime number generation using `sympy`  
✅ GCD and modular inverse implemented from scratch  
✅ Explanatory code comments and modular functions  

---

## 🚀 How It Works

### 1. Key Generation
- Two large prime numbers `p` and `q` are generated.
- Compute `N = p * q` and Euler’s totient `φ(N) = (p - 1)(q - 1)`.
- A public exponent `e` is chosen such that `gcd(e, φ(N)) = 1`.
- The private key `d` is computed as the modular inverse of `e` mod `φ(N)`.

### 2. Message Encryption
Each character is:
- Converted to an integer using `ord()`.
- Encrypted using RSA formula:
  ```math
c ≡ m^e mod N

### 3. Message Decryption
Each encrypted integer is:
- Decrypted using the formula:  
```math
m ≡ c^d mod N
- Converted back to a character using `chr()`.

---

## 💻 Sample Usage

```python
msg = "hello"
converted_msg= convert_msg(msg)
cipher = crypt_msg(converted_msg)
print("Encrypted:", cipher)

plain = decrypt_msg(cipher)
print("Decrypted:", plain)  # Output: "hello"
