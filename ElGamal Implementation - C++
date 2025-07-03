// Tahim Bhuiya
// ElGamal Implementation in C++

#include <iostream>     // For input/output stream
#include <vector>       // For using dynamic arrays
#include <cmath>        // For mathematical functions like pow(), sqrt()
#include <ctime>        // For seeding random number generator with time
#include <cstdlib>      // For rand(), srand()
#include <string>       // For string handling
#include <tuple>        // For returning multiple values from a function using std::tuple

using namespace std;    // To avoid writing std:: repeatedly (e.g., std::cout -> cout)


// Greatest Common Divisor (GCD) using the Euclidean Algorithm
// Recursively computes the GCD of two integers a and b.
// If b is 0, a is the GCD. Otherwise, recursively call gcd(b, a % b).
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}


// Modular Exponentiation using the Square-and-Multiply method
// Computes (a^b) mod c efficiently without overflow
// a: base, b: exponent, c: modulus
int power(int a, int b, int c) {
    int result = 1;
    a = a % c;  // Ensure base is within modulus

    while (b > 0) {
        // If b is odd, multiply result with current base
        if (b % 2 == 1)
            result = (result * a) % c;

        // Square the base and reduce it modulo c
        a = (a * a) % c;

        // Divide exponent by 2
        b /= 2;
    }

    return result;
}


// Fermat Primality Test (basic version using base 2)
// Checks if a number p is probably prime using Fermat's Little Theorem
// Returns true if 2^(p-1) mod p == 1, which is a necessary condition for prime
// Note: This method can falsely identify Carmichael numbers as primes
bool is_prime(int p) {
    if (p < 2 || p % 2 == 0) return false; // Eliminate numbers < 2 and even numbers > 2
    return power(2, p - 1, p) == 1;        // Fermat's test with base 2
}

// Generate a random prime number of approximately 'bits' bits using Fermat's test
// The generated number lies in the range [2^(bits-1), 2^bits - 1]
// It repeatedly generates random numbers in that range until a probable prime is found
int generate_prime(int bits) {
    int min_val = 1 << (bits - 1);            // Minimum value with 'bits' bits (e.g., 100...0 in binary)
    int max_val = (1 << bits) - 1;            // Maximum value with 'bits' bits (e.g., 111...1 in binary)
    int p;

    do {
        p = min_val + rand() % (max_val - min_val); // Generate random number in the range [min_val, max_val)
    } while (!is_prime(p));                          // Repeat until a number passes the primality test

    return p;
}

// Generator for a Prime Field
// Attempts to find a primitive root (generator) modulo p
// This simplified check assumes (p-1) has small prime factors like 2 and 3
// A valid generator g must satisfy: g^((p-1)/q) mod p ≠ 1 for all prime factors q of (p-1)
int find_generator(int p) {
    for (int g = 2; g < p; ++g) {
        // Check if g is not a root of unity for factors 2 and 3
        if (power(g, (p - 1) / 2, p) != 1 && power(g, (p - 1) / 3, p) != 1)
            return g;  // Found a valid generator
    }
    return -1;  // No generator found (shouldn’t usually happen for safe primes)
}

// Key Generation for ElGamal Cryptosystem
// Generates a prime q, a generator g, a private key, and the corresponding public key
// Returns a tuple: (q, g, private_key, public_key)
tuple<int, int, int, int> generate_keys(int bits) {
    int q = generate_prime(bits);             // Generate a prime number q of given bit size
    int g = find_generator(q);                // Find a generator g for the prime field modulo q

    int min_key = 1 << (bits / 2);            // Minimum value for the private key (half the bit size of q)
    int private_key = min_key + rand() % (q - min_key);  // Random private key in range [min_key, q)
    int public_key = power(g, private_key, q);           // Compute public key: g^private_key mod q

    return {q, g, private_key, public_key};   // Return all key components as a tuple
}

// Modular Inverse using the Extended Euclidean Algorithm
// Finds the modular inverse of a modulo m, i.e., a number x such that (a * x) % m == 1
// Assumes a and m are coprime (GCD = 1)
int modinv(int a, int m) {
    int m0 = m;       // Store original modulus value
    int t, q;         // Temporary variables for calculations
    int x0 = 0, x1 = 1; // Initialize coefficients for Bézout's identity

    // Apply Extended Euclidean Algorithm
    while (a > 1) {
        q = a / m;         // Quotient
        t = m;
        m = a % m;         // Remainder becomes new m
        a = t;             // Previous m becomes new a

        t = x0;
        x0 = x1 - q * x0;  // Update x0 and x1 using the quotient
        x1 = t;
    }

    // Ensure x1 is positive
    return x1 < 0 ? x1 + m0 : x1;
}

// Encrypt function for ElGamal Cryptosystem
// Encrypts a string message character-by-character using public parameters (q, g, public_key)
// Returns a vector of ciphertext pairs (p1, p2) for each character
vector<pair<int, int>> encrypt(const string &msg, int q, int g, int public_key) {
    int k = 2 + rand() % (q - 2);         // Generate random session key k ∈ [2, q-1]
    int s = power(public_key, k, q);      // Shared secret: s = public_key^k mod q
    vector<pair<int, int>> ciphertext;   // Vector to store encrypted character pairs

    for (char ch : msg) {
        int p1 = power(g, k, q);          // p1 = g^k mod q
        int p2 = ((int)ch * s) % q;       // p2 = m * s mod q, where m is the character ASCII value
        ciphertext.emplace_back(p1, p2);  // Append encrypted pair to ciphertext
    }

    return ciphertext;
}

// Decrypt function for ElGamal Cryptosystem
// Decrypts a vector of ciphertext pairs using the private key and modulus q
// Returns the recovered plaintext string
string decrypt(const vector<pair<int, int>> &ciphertext, int q, int private_key) {
    string plaintext;

    // Recover the shared secret using the first p1 and the private key
    int h = power(ciphertext[0].first, private_key, q);  // h = p1^private_key mod q

    // Compute the modular inverse of the shared secret
    int inv_h = modinv(h, q);  // inv_h = h⁻¹ mod q

    // Decrypt each pair (p1, p2)
    for (auto &pair : ciphertext) {
        int ch = (pair.second * inv_h) % q;             // m = p2 * h⁻¹ mod q
        plaintext += static_cast<char>(ch);             // Convert ASCII value to character
    }

    return plaintext;
}

int main() {
    srand(time(0));  // Seed the random number generator with current time

    int bits;
    cout << "Enter the key size in bits (preferably 8, 12, 16): ";

    // Input validation: ensure user enters a positive integer for key size
    while (!(cin >> bits) || bits <= 0) {
        cout << "Please enter a valid positive integer: ";
        cin.clear();              // Clear error flag on cin
        cin.ignore(1000, '\n');   // Discard invalid input from buffer
    }
    // Generate ElGamal key components (q, g, private_key, public_key)
    auto [q, g, private_key, public_key] = generate_keys(bits);

    // Display the generated keys and prime number
    cout << "\nPrime Number (q): " << q;
    cout << "\nPublic Key: " << public_key;
    cout << "\nPrivate Key: " << private_key;

    cin.ignore();  // Clear any leftover newline character from previous input

    string plaintext;
    cout << "\nEnter the plaintext: ";
    getline(cin, plaintext);  // Read full line of text including spaces as the message to encrypt

    // Encrypt the plaintext message using ElGamal encryption
    auto ciphertext = encrypt(plaintext, q, g, public_key);

    // Display the ciphertext as pairs (p1, p2) for each character
    cout << "\nCiphertext: ";
    for (auto &pair : ciphertext) {
        cout << "(" << pair.first << ", " << pair.second << ") ";
    }

    // Decrypt the ciphertext using the private key
    string decrypted = decrypt(ciphertext, q, private_key);

    // Display the recovered plaintext
    cout << "\nDecrypted Plaintext: " << decrypted << endl;

    return 0; // Indicate successful program termination
}