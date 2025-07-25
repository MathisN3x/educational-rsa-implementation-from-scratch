# educational-rsa-implementation-from-scratch
# 🔐 RSA Cryptosystem in Python

This project demonstrates a **basic but functional implementation of RSA** — one of the most well-known public-key cryptosystems — using Python and `sympy` for prime number generation.

> ⚠️ **Disclaimer**: This code is educational and should not be used in production. It lacks padding schemes and cryptographic safety measures like OAEP.

---

## 📚 Overview

The notebook walks through the 3 core stages of RSA:

1. **Key Generation**: Selects two large prime numbers and computes the public and private keys.
2. **Encryption**: Converts a text message into integers and encrypts them using the RSA public key.
3. **Decryption**: Uses the private key to recover the original message.

---

## 🛠️ Technologies Used

- Python 3.x
- [`sympy`](https://www.sympy.org/) for generating large prime numbers
- Jupyter Notebook for an interactive and educational walkthrough

---

## 🧪 Example Usage

```python
message = "hello"
ciphertext = encrypt_msg(message, e, N)
plaintext = decrypt_msg(ciphertext, d, N)
print(plaintext)  # Output: "hello"
