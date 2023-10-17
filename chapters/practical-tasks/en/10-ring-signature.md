# 10. Ring-signature
One of the CryptoNote standards describes the algorithm of traceable ring signatures. This approach allows a user
to sign a transaction while preserving anonymity, as the verifier can confirm that the signature was produced by
one of the group members without being able to find out who exactly. To prevent double-spending attacks, a ring
signature mechanism was used, which utilizes a private key image.

```
Pseudo-code:

Signature input:
M 		- message
A[0...n-1] 	- public keys of ring participants,
а[i]		- private signer key,
А[i] 		- public signer key.

Procedure generate_signature (M, A[1], A[2], ..., A[n], i, a[i]):
	I <- a[i]*H(A[i])	// Private key image
    c[j], r[j] [j=0..n-1, j!=i] <- random
    k <- random
    For j <- 0..n-1, j!=i
		X[j] <- c[j]*A[j]+r[j]*G
		Y[j] <- c[j]*I+r[j]*H(A[j])
    X[i] <- k*G
    Y[i] <- k*H(A[i])
    c[i] <- H(H(M) || X[0] || Y[0] || X[1] || Y[1] || ... || X[n-1] || Y[n-1]) - Sum[j=0..n-1, j!=i](c[j])
    r[i] <- k-a[i]*c[i]

    Return (I, c[0] || r[0] || c[1] || r[1] || ... || c[n-1] || r[n-1])

Procedure verify_signature(M, A[0], A[1], ..., A[n-1], I, c[0], r[0], c[1], r[1], ..., c[n-1], r[n-1]):
    For i <- 0..n-1
		X[i] <- c[i]*A[i]+r[i]*G
		Y[i] <- c[i]*I+r[i]*H(A[i])
    If H(H(M) || X[0] || Y[0] || X[1] || Y[1] || ... || X[n-1] || Y[n-1])  == Sum[i=0..n-1](c[i])
		Return "Correct"
    Else
		Return "Incorrect"
}
```
You can learn more about CryptoNote standards at the following link:
https://cryptonote.org/standards/

This example from our worker Mikhail's implementation generates a ring with random open keys and signs a message
using a random subset of the ring. The signature can be verified using the open keys of the users in the subset.
Note that this implementation does not include any key management or distribution mechanisms important for 
real-world use of the digital signature algorithm.

🤖

```go
package main

import (
	"crypto/ecdsa"
	"crypto/elliptic"
	"crypto/rand"
	"crypto/sha256"
	"fmt"
	"math/big"
)

const ringSize = 4

type Ring struct {
	Keys []*ecdsa.PublicKey
}

func (r *Ring) Sign(message []byte) ([]byte, error) {
	// Select a random subset of the ring
	subset := make([]*ecdsa.PublicKey, ringSize)
	for i := 0; i < ringSize; i++ {
		subset[i] = r.Keys[rand.Intn(len(r.Keys))]
	}

	// Generate a random private key for the signature
	privKey, err := ecdsa.GenerateKey(elliptic.P256(), rand.Reader)
	if err != nil {
		return nil, err
	}

	// Compute the message hash
	hash := sha256.Sum256(message)

	// Compute the partial signature for each key in the subset
	partialSigs := make([]*big.Int, ringSize)
	for i := 0; i < ringSize; i++ {
		r, s, err := ecdsa.Sign(rand.Reader, privKey, hash[:])
		if err != nil {
			return nil, err
		}
		partialSigs[i] = new(big.Int).Set(r)
	}

	// Combine the partial signatures into a single signature
	sig := new(big.Int)
	for i := 0; i < ringSize; i++ {
		sig.Add(sig, partialSigs[i])
	}

	// Encode the signature as a byte array
	sigBytes := sig.Bytes()

	return sigBytes, nil
}

func (r *Ring) Verify(message, signature []byte) bool {
	// Decode the signature into a big.Int
	sigInt := new(big.Int).SetBytes(signature)

	// Verify the signature using the public keys in the subset
	for i := 0; i < ringSize; i++ {
		r, s := sigInt.DivMod(sigInt, elliptic.P256().Params().N, new(big.Int))
		if ecdsa.Verify(r, s, r.Keys[i], message) {
			return true
		}
	}

	return false
}

func main() {
	// Generate a ring with random public keys
	r := &Ring{
		Keys: make([]*ecdsa.PublicKey, ringSize),
	}
	for i := 0; i < ringSize; i++ {
		key, err := ecdsa.GenerateKey(elliptic.P256(), rand.Reader)
		if err != nil {
			panic(err)
		}
		r.Keys[i] = &key.PublicKey
	}

	// Sign and verify a message using the ring
	message := []byte("Hello, world!")
	signature, err := r.Sign(message)
	if err != nil {
		panic(err)
	}
	valid := r.Verify(message, signature)
	fmt.Printf("Valid signature: %v\n", valid)
}

```

> However, it should be noted that the personal key of the signer should actually be taken as a function parameter,
instead of generating it inside the function. The signing function should take the already-formed ring as a parameter,
instead of taking all the open keys and choosing random ones during the signing. It should also be noted that key
pair generation is better implemented as a separate function, as well as ring formation.

✯ Your task is to try to implement the one-time ring signature algorithm, considering the comments on the demonstration code made by our robot Mykhailo.
