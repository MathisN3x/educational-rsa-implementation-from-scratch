# Educational RSA Implementation — From Scratch 🔐

This repository contains a fully manual, educational RSA implementation in Python. I developed this project the afternoon after my professor mentioned RSA in class — it was a small, hands‑on exercise completed in one afternoon to explore and understand the algorithm step by step.

> ⚠️ This implementation is strictly for educational purposes. It omits cryptographic padding, constant‑time operations, and other important security hardening required for real-world use.

---

## What you'll learn

- The mathematics behind RSA: Euler’s theorem, modular inverses and why they work.  
- How to implement each step of RSA manually in Python (prime generation, key creation, encryption, decryption).  
- How to encode and decode text to integers for use with RSA.  
- Common pitfalls and how to interpret results when something goes wrong.

---

## Files

| File | Description |
|------|-------------|
| `RSA_test.ipynb` | Jupyter notebook with code, demonstrations, and step‑by‑step explanations in English |

Note: The notebook has been expanded with more detailed English explanations and inline commentary to make the math and code easier to follow.

---

## Mathematical overview

Key generation and encryption/decryption formulas (properly encoded):

- Choose two distinct prime numbers p and q.
- Compute the modulus:
  $$N = p \cdot q$$
- Compute Euler’s totient:
  $$\varphi(N) = (p - 1)(q - 1)$$
- Choose a public exponent e such that:
  $$\gcd(e, \varphi(N)) = 1$$
- Compute the private exponent d as the modular inverse of e modulo φ(N):
  $$d \equiv e^{-1} \pmod{\varphi(N)}$$

Encryption of a message integer m (with 0 ≤ m < N):
$$c \equiv m^{e} \pmod{N}$$

Decryption of a ciphertext integer c:
$$m \equiv c^{d} \pmod{N}$$

In the code, each character is converted to an integer (e.g., via ord/chr for ASCII) before applying the above modular exponentiation formulas.

---

## Features

- Manual RSA key generation with readable code.  
- Encryption and decryption of ASCII-based messages.  
- Prime number generation using `sympy` for convenience.  
- Greatest common divisor (GCD) and modular inverse implemented from scratch (educational).  
- Clear, explanatory comments and modular functions.  
- Expanded notebook with additional, detailed English explanations and worked examples.

---

## Example usage

```python
msg = "hello"
converted_msg = convert_msg(msg)   # convert characters to integers
cipher = crypt_msg(converted_msg)  # encrypt using public key
print("Encrypted:", cipher)

plain = decrypt_msg(cipher)        # decrypt using private key
print("Decrypted:", plain)         # -> "hello"
```

---

## Improvements made

- Added a clear note that the idea for this repository originated in class after RSA was mentioned by my professor, and implemented the core demonstration in one afternoon as a learning exercise.  
- Properly encoded and displayed the core mathematical formulas using LaTeX in the README for clarity.  
- Expanded the `RSA_test.ipynb` notebook with more detailed English explanations and step‑by‑step commentary to help readers understand each piece of the implementation.

---

## Contributing

Contributions are welcome for educational clarifications, improved explanations, or suggestions to add secure, production‑ready primitives (with appropriate cryptographic padding and libraries). Please open an issue or submit a pull request.

---

If you'd like, I can create a commit with this revised README and/or open a pull request. What would you prefer?
