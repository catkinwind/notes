This is from https://nakedsecurity.sophos.com/2013/11/20/serious-security-how-to-store-your-users-passwords-safely/

The recommanded attempt is "Hash Stretching". We can use PBKDF2, bcrypt or scrypt.

* Introduce to PBKDF2

The poster recommand to use PBKDF2 with HMAC-SHA-256. HMAC-SHA-256 processes as following,

- Take a random key or salt K, and flip some bits, giving K1
- Compute the SHA-256 hash of K1 plus the input, giving H1
- Flip a different set of bits in K, giving K2
- Compute the SHA-256 hash of K2 pulus the input, giving H2
- Continue to interate until a given count, like 10000.

PBKDF2 with HMAC-SHA-256 works like,

we feed the user`s password and salt into HMAC-SHA-256 and make the first of the 10, 000 loops

Then we feed the password and the previously-computed HMAC hash back into HMAC-SHA-256 for the remaining
9999 times round the loop. Every time round the loop, the latest output is XORed with the previous one to keep
to keep a running "accumulator", when we are done, the accumulator becomes the final PBKDF2 hash.


    password              salt
        \                  /
            HMAC-SHA-256
                   \
      password     HMAC-1
          \          /   \
           HMAC-SHA-256  XOR
                      \   \
        password       HMAC-2
              \         /   \
              HMAC-SHA-256  XOR
                         \    \
                          HMAC-3  (final)
