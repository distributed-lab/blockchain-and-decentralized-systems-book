# 9. Schnorr signatures
Schnorr signatures are a type of digital signature that support the aggregation of public keys and signature values
in such a way as to make multi-signatures compact and anonymous (a single signature is indistinguishable from a 
signature generated by a potentially infinite number of participants).

```
Pseudo-code:

KeyGen(){
d <= random ∈ [1, n - 1].
Q = d x P.

// additional verifications
Checking that Q ≠ 0.
Checking that xQ and yQ are properly represented elements of Fq.
Checking that Q is on the elliptic curve defined by a and b.
Checking that nQ = Q.

If any of these checks fail the public key Q is invalid, otherwise Q is valid.

Sign(m, d){
	r <= random
	R := scalarMult(P, *r)
	
	r = r + H(X,R,m)*x

	signature.R = R
	signature.S = *r

	return
}

Verify(r, s, Q){
	sP ?= R + H(X,R,m)Q 
}
```

The Schnorr multi-signature algorithm has the following form:

```
Pseudo-code:

Sign(){
    L = H(X1,X2,…)
    X = sum(H(L,Xi)*Xi)
    ri - rand 
    Ri = ri*G
    R = sum(Ri)

    Each signer computes si = ri + H(X,R,m)*H(L,Xi)*xi

    s is the sum of the si values
}

Verify(){
    sG ?= R + H(X,R,m)*X
}
```

More information about the mechanism of Schnorr signatures and examples of their software implementation can be found at
the following link:  
https://tlu.tarilabs.com/cryptography/introduction-schnorr-signatures


Here is a simple implementation of Schnorr signatures in GoLang that uses the cryptographic hash function SHA-256.
This example has limitations, such as using a very simple curve instead of a secure elliptic curve. It is only meant
to illustrate the concept of Schnorr signatures and is not suitable for real-world applications.

🤖

```go
package main

import (
	"crypto/rand"
	"crypto/sha256"
	"fmt"
	"math/big"
)

func main() {
	// Key generation
	privKey, pubKey := generateKeys()
	fmt.Println("Keys:")
	fmt.Println("  Private key:", privKey)
	fmt.Println("  Public key:", pubKey)
	// Signature creation
	message := "This is a message to sign"
	signature := sign(message, privKey)
	fmt.Println("Signature:")
	fmt.Println("  Message:", message)
	fmt.Println("  Signature:", signature)
	// Signature verification
	isValid := verify(message, signature, pubKey)
	fmt.Println("Signature verification:", isValid)
}

func generateKeys() (x, Y *big.Int) {
	curve := getCurve()
	x, err := rand.Int(rand.Reader, curve.n)
	if err != nil {
		panic(err)
	}
	Y = new(big.Int).Mul(x, curve.g)
	Y.Mod(Y, curve.p)
	return
}

func sign(message string, x *big.Int) *big.Int {
	curve := getCurve()
	e := hashToInt(message)
	k, err := rand.Int(rand.Reader, curve.n)
	if err != nil {
		panic(err)
	}
	r := new(big.Int).Mul(k, curve.g)
	r.Mod(r, curve.p)
	s := new(big.Int).Mul(x, r)
	s.Add(s, new(big.Int).Mul(k, e))
	s.Mod(s, curve.n)
	return s
}

func verify(message string, s, Y *big.Int) bool {
	curve := getCurve()
	e := hashToInt(message)
	ev := new(big.Int).Mul(e, Y)
	ev.Mod(ev, curve.p)
	sv := new(big.Int).Mul(s, curve.g)
	sv.Mod(sv, curve.p)
	return ev.Cmp(sv) == 0
}

func hashToInt(message string) *big.Int {
	hash := sha256.Sum256([]byte(message))
	return new(big.Int).SetBytes(hash[:])
}

func getCurve() *curve {
	return &curve{
		p: new(big.Int).SetInt64(101),
		n: new(big.Int).SetInt64(101),
		g: new(big.Int).SetInt64(2),
	}
}

type curve struct {
	p, n, g *big.Int
}
```

As can be seen from this implementation, this is a regular single signature using the Schnorr algorithm. Our assistant
Mykhailo did not perform any key aggregation or signature aggregation. Therefore, this code does not implement a
multi-signature.

✯ Try to implement the Schnorr signature using this section's material and  focus on implementing key and signature
aggregation to obtain code that implements multi-signatures.