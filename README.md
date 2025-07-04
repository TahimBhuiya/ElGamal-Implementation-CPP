
# 🔐 ElGamal Encryption and Decryption in C++  
**By Tahim Bhuiya**

This is a C++ implementation of the **ElGamal public-key cryptosystem**, which encrypts and decrypts text using asymmetric cryptography based on modular arithmetic and discrete logarithms.

---

## 📜 Overview  
ElGamal is an asymmetric-key encryption algorithm used for secure data exchange. This project simulates ElGamal by:

- Generating a large prime number `q`  
- Finding a generator `g` for the finite field modulo `q`  
- Generating a private key and computing the corresponding public key  
- Performing asymmetric encryption using a random session key  
- Decrypting ciphertext using modular inverse and private key  

---

## ▶️ Usage  

Compile and run the C++ program:  
```bash
g++ "ElGamal Implementation - C++.cpp" -o ElGamal && ./ElGamal
```

Then:  
- Enter the key size in bits (e.g., 8, 12, 16)  
- Enter the plaintext message to encrypt  

The program will output:  
- The generated prime number `q`  
- Public and private keys  
- Encrypted ciphertext as pairs of integers `(p1, p2)`  
- Decrypted plaintext  

---
## 🧠 Code Description  

| Function                      | Purpose                                                                   |
|-------------------------------|---------------------------------------------------------------------------|
| `gcd(a, b)`                   | Computes greatest common divisor using Euclidean algorithm                |
| `power(a, b, c)`              | Efficient modular exponentiation (square-and-multiply)                    |
| `is_prime(p)`                 | Fermat primality test to check if `p` is probably prime                  |
| `generate_prime(bits)`        | Generates a random prime number with approximately `bits` bits           |
| `find_generator(p)`           | Finds a primitive root (generator) modulo prime `p`                      |
| `generate_keys(bits)`         | Generates `(q, g, private_key, public_key)` for ElGamal                   |
| `modinv(a, m)`                | Computes modular inverse of `a` modulo `m` using extended Euclidean algorithm |
| `encrypt(msg, q, g, public_key)` | Encrypts a plaintext string into a vector of ciphertext pairs           |
| `decrypt(ciphertext, q, private_key)` | Decrypts ciphertext pairs into the original plaintext string        |
---

## 🔐 Key Concepts  

| Concept         | Description                                                                      |
|-----------------|----------------------------------------------------------------------------------|
| Prime `q`       | Large prime number used as the modulus                                          |
| Generator `g`   | Primitive root modulo `q`                                                       |
| Private Key     | Randomly chosen secret integer                                                  |
| Public Key      | Computed as `g^private_key mod q`                                               |
| Session Key `k` | Random key generated per encryption session                                    |
| Shared Secret   | Calculated as `(public_key)^k mod q`, used to mask plaintext characters         |

---