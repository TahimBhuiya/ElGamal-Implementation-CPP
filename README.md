
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