# 8. ECDSA Signature Algorithm
The digital signature algorithm based on elliptic curves (ECDSA) offers a variant of the digital signature algorithm
(DSA) that uses cryptography based on elliptic curves.

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

    If any of these checks fail the public key Q is invalid; otherwise Q is valid. 

Sign(m, d){
	k <= random ∈ [1, n - 1].
    k x P = (x1, y1)
    r = x1 mod n.
    if r = 0 then go to start.
    s = pow(k, -1)(H(m) + d • r) mod n.
    if s = 0 go to start.
    Signature (r, s).
}

Verify(r, s, Q){
	Verify that r and s are integers in the interval [1,n-1].
    c = s-1 mod n and H(m)  
    u1 = H(m) • c mod n
    u2 = r • c mod n
    u1 x P + u2 x Q = (x0, y0)
    v = x0 mod n
	if v = r -> return true
}
```

You can find the text of the standard and the test vectors at the following link:
https://datatracker.ietf.org/doc/html/rfc6979

Our student Mykhailo proposes such an implementation with artificial intelligence, but this implementation is
intended only for educational purposes and should not be used in production systems, as it lacks proper error
handling, security and productivity optimization. Instead, use widely used and proven cryptography libraries.

🤖
```go
package main

import (
	"crypto/elliptic"
	"crypto/rand"
	"math/big"
)

func main() {
	// Define the curve to use
	curve := elliptic.P256()

	// Generate a private key
	privateKey, x, y, err := elliptic.GenerateKey(curve, rand.Reader)
	if err != nil {
		panic(err)
	}

	// Define a message to be signed
	message := []byte("Hello, World!")

	// Sign the message using the private key
	r, s, err := sign(curve, privateKey, message)
	if err != nil {
		panic(err)
	}

	// Verify the signature using the public key
	publicKey := append(x.Bytes(), y.Bytes()...)
	if verify(curve, publicKey, message, r, s) {
		println("Signature verified")
	} else {
		println("Signature not verified")
	}
}

func sign(curve elliptic.Curve, privateKey []byte, message []byte) (r, s *big.Int, err error) {
	hash := curve.Hash(message)
	k, err := rand.Int(rand.Reader, curve.Params().N)
	if err != nil {
		return nil, nil, err
	}
	rx, ry := curve.ScalarBaseMult(k.Bytes())
	r = rx.Mod(rx, curve.Params().N)
	kInv := new(big.Int).ModInverse(k, curve.Params().N)
	s = new(big.Int).Mul(privateKey, r)
	s.Add(s, hash)
	s.Mul(s, kInv)
	s.Mod(s, curve.Params().N)
	return
}

func verify(curve elliptic.Curve, publicKey []byte, message []byte, r, s *big.Int) bool {
	if r.Cmp(curve.Params().P) >= 0 || s.Cmp(curve.Params().N) >= 0 {
		return false
	}
	hash := curve.Hash(message)
	wx, wy := curve.ScalarBaseMult(s.Bytes())
	wx.Mul(wx, r)
	wx.Mod(wx, curve.Params().P)
	rx, ry := curve.ScalarMult(curve.Params().Gx, curve.Params().Gy, hash.Bytes())
	rx.Sub(rx, wx)
	rx.Mod(rx, curve.Params().P)
	return rx.Cmp(r) == 0
}
```
✯ Try to implement the ECDSA algorithm using the material from this section and the demonstration implementation
from our colleague Mykhailo, but as an additional task, add serialization of keys and signatures in a HEX
string or Base64.


