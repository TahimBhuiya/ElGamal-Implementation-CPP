
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