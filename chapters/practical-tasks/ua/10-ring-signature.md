# 10. Одноразовий кільцевий підпис
Один із стандартів CryptoNote описує алгоритм одноразового кільцевого підпису (traceable ring signatures). Такий підхід 
дозволяє користувачеві підписати транзакцію і зберегти при цьому анонімність, тому що верифікатор може переконатися, що 
підпис був вироблений одним із членів групи, не маючи можливості дізнатися, ким саме. Для запобігання атаки подвійної 
витрати було вирішено використати механізм кільцевого підпису, який використовує private key image.

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
Детальніше зі стандартами CryptoNote можна ознайомитися за посиланням:
https://cryptonote.org/standards/

✯ Спробуйте реалізувати алгоритм одноразового кільцевого підпису, використовуючи матеріал цього розділу.

Цей приклад від нашого робота Михайла реалізації генерує кільце з випадковими відкритими ключами та підписує 
повідомлення за допомогою випадкової підмножини кільця. Підпис можна перевірити за допомогою відкритих ключів 
користувачів у підмножині. Зауважте, що ця реалізація не включає жодних механізмів керування або розподілу ключів, 
які є важливими для реального використання алгоритму цифрового підпису.

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

> Однак, треба зауважити, що насправді особистий ключ того, хто підписує, треба приймати як параметр функції, замість 
того щоб генерувати його всередині функції. Функція підпису має приймати як параметр вже сформоване кільце, 
замість того щоб приймати всі відкриті ключі й обирати випадкові під час підпису. Також треба відмітити, що генерацію 
ключових пар краще реалізовувати окремою функцією, так само як і формування кільця.
