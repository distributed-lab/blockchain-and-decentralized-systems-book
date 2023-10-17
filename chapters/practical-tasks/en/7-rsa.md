# 7. RSA
The RSA encryption algorithm is an asymmetric encryption algorithm. Today, this algorithm is often used to establish
a secure channel between users (forming a shared secret and encrypting it using RSA).
The sequence of steps of the RSA algorithm (key generation):

1. Two large prime numbers _**p**_ and **_q_** are selected;
2. Calculate: **_n = p ⋅ q, m = (p-1) ⋅ (q-1)_**;
3. A random number **_d_** is chosen, which is coprime with **_m_**;
4. Determine a number **_e_** for which the expression **_(e ⋅ d) mod (m) = 1_** is true.
5. The numbers **_e_** and **_n_** are the parts of the public key, and the numbers **_d_** and **_n_** are the private
   key parts.


Messages are encrypted with a public key and decrypted with a private key. The pair of numbers of the private key
is kept secret.

Encryption and decryption of information using the RSA algorithm.
1. The original text is divided into blocks, each of which can be represented as a number **_M(i)_**;
2. The encryption of the text is performed as follows: **_C(i) = (M(i)e) mod n_**;
3. The decryption of the message is performed as follows: **_M(i) = (C(i)d) mod n_**.

```
Pseudo-code:

KeyGen(){
    P and Q <= primary big numbers.
    n = P*Q.
 	e <= integer value, not be a factor of n, 1<e<Φ(n), Φ(n) = (P-1)(Q-1)
    d = (k*Φ(n) + 1) / e , for some integer k

	PrivateKey (d, n)
	PublicKey (e, n)
}

Encrypt(message, PublicKey){
	return powmod(message, e, n)
}

Decrypt(ciphertext, PrivateKey){
	return powmod(ciphertext, d, n)
}
```
✯ Try to implement the RSA algorithm using the material in this section.

You can find the actual text of the standard and test vectors at the following link:
https://datatracker.ietf.org/doc/html/rfc3447

Creating your own implementation of the RSA algorithm can be a difficult task, especially without using built-in
libraries. One possible implementation may look like the following code, which generates RSA keys, encrypts and
decrypts plaintext. Note that this example is simplified and unsuitable for  real applications. It is recommended
to use libraries that have undergone security and optimization audit for real applications.

🤖

```go
ppackage main

import (
"fmt"
"math/big"
"math/rand"
"time"
)

func main() {
//  Key generation
p, q := generatePrimes(128)
n := new(big.Int).Mul(p, q)
e := big.NewInt(65537)
phi := new(big.Int).Mul(new(big.Int).Sub(p, big.NewInt(1)), new(big.Int).Sub(q, big.NewInt(1)))
d := new(big.Int).ModInverse(e, phi)

fmt.Println("Keys:")
fmt.Println("  Public:", n, e)
fmt.Println("  Private:", n, d)
// Encryption
plaintext := big.NewInt(123456)
ciphertext := encrypt(plaintext, e, n)
fmt.Println("Encryption:")
fmt.Println("  Plaintext:", plaintext)
fmt.Println("  Ciphertext:", ciphertext)
// Decryption
decrypted := decrypt(ciphertext, d, n)
fmt.Println("Decryption:")
fmt.Println("  Ciphertext:", ciphertext)
fmt.Println("  Plaintext:", decrypted)
}

func encrypt(m, e, n *big.Int) *big.Int {
return new(big.Int).Exp(m, e, n)
}

func decrypt(c, d, n *big.Int) *big.Int {
return new(big.Int).Exp(c, d, n)
}

func generatePrimes(bits int) (p, q *big.Int) {
randSource := rand.New(rand.NewSource(time.Now().UnixNano()))
for {
p, _ = rand.Prime(randSource, bits)
q, _ = rand.Prime(randSource, bits)
if p.Cmp(q) != 0 {
break
}
}
return
}

func gcd(a, b *big.Int) *big.Int {
for b.Sign() > 0 {
a, b = b, new(big.Int).Mod(a, b)
}
return a
}
```

> Apparently, this code works with constant key and module lengths, so for attempting its implementation, we
> advise implementing support for keys and modules of arbitrary length. Moreover, this code sets the public key
> as a constant, but in real use cases, a new key should be generated every time. In fact, key and module generation
> should be allocated  into a separate function. Also, all values (keys, module, message) should be stored and 
> passed between functions as arrays of bytes. This is important so that different implementations can work with 
> the same keys and messages with the ability to export and import keys. Note that our robot Mykhailo implemented
> only the encryption algorithm, but you - as our readers and students - can extend this implementation by adding
> a digital signature.
