# 9. Підписи Шнорра
Підписи Шнорра – це вид цифрових підписів, які підтримують агрегацію відкритих ключів та значень підписів, таким чином, 
щоб зробити мультипідпис компактним та анонімним (одиничний підпис жодним чином не відрізняється від підпису, 
згенерованого потенційно нескінченною кількістю учасників).

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

Алгоритм мультипідпису Шнорра має такий вигляд:

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

Детальніше з механізмом роботи підписів Шнорра та прикладами програмної реалізаціʼ можна ознайомитися за посиланням: 
https://tlu.tarilabs.com/cryptography/introduction-schnorr-signatures

✯ Спробуйте реалізувати підпис Шнорра, використовуючи матеріал цього розділу.

Ось проста реалізація підписів Шнорра на GoLang, що використовує криптографічну хеш-функцію SHA-256. Цей приклад має 
деякі обмеження, такі як використання дуже простої кривої замість безпечної еліптичної кривої. Він служить лише для 
ілюстрації концепції підписів Шнорра та не підходить для реальних застосунків.

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
	// Генерація ключів
	privKey, pubKey := generateKeys()
	fmt.Println("Ключі:")
	fmt.Println("  Приватний ключ:", privKey)
	fmt.Println("  Відкритий ключ:", pubKey)

	// Створення підпису
	message := "Це повідомлення для підпису"
	signature := sign(message, privKey)
	fmt.Println("Підпис:")
	fmt.Println("  Повідомлення:", message)
	fmt.Println("  Підпис:", signature)

	// Перевірка підпису
	isValid := verify(message, signature, pubKey)
	fmt.Println("Перевірка підпису:", isValid)
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