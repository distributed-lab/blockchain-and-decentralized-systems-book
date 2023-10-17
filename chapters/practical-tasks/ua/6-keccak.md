# 6. Алгоритм Keccak
Keccak (SHA-3) – алгоритм гешування змінної розрядності. Геш-функція побудована на основі конструкції криптографічної 
губки, в якій дані спочатку вбираються в губку, при цьому вихідне повідомлення M зазнає багатораундових перестановок f, 
а потім результат Z «віджимається» з губки. На етапі «вбирання» блоки повідомлення сумуються за модулем 2 з підмножиною 
стану, після чого весь стан перетворюється за допомогою функції перестановки f. На етапі «віджимання» вихідні блоки 
зчитуються з одного і того ж підмножини стану, зміненого функцією перестановок f.

Основою функції стиснення алгоритму є функція f, що виконує перемішування внутрішнього стану алгоритму. Стан A 
представляється у вигляді масиву 5×5, елементами якого є 64-бітові слова, ініціалізовані нульовими бітами (тобто розмір 
стану становить 1600 бітів). Функція f виконує 24 раунди, у кожному з яких виробляються покрокові перетворення θ, ρ, π, 
χ, ι.

На початку перетворення виконується padding (заповнення/додавання). Padding необхідний для приведення вихідного 
повідомлення до рядка фіксованої довжини. Потім виконується функція «Absorb». Ця функція призначена для розбиття 
вхідного (що пройшла через padding) рядки на складові, які будуть послідовно використовуватися для наступних 
перетворень:
- θ перетворення; 
- ρ і π перетворення; 
- χ перетворення; 
- ι перетворення. 
- Після цього виконується процедура Squeezing (стиснення / видавлювання). Ця процедура необхідна для обчислення 
- кінцевого геш-значення.

```
Pseudo-code:

Keccak[r,c](Mbytes || Mbits) {
  # Padding
  d = 2^|Mbits| + sum for i=0..|Mbits|-1 of 2^i*Mbits[i]
  P = Mbytes || d || 0x00 || … || 0x00
  P = P xor (0x00 || … || 0x00 || 0x80)

  # Initialization
  S[x,y] = 0,                               for (x,y) in (0…4,0…4)

  # Absorbing phase
  for each block Pi in P
    S[x,y] = S[x,y] xor Pi[x+5*y],          for (x,y) such that x+5*y < r/w
    S = Keccak-f[r+c](S)

  # Squeezing phase
  Z = empty string
  while output is requested
    Z = Z || S[x,y],                        for (x,y) such that x+5*y < r/w
    S = Keccak-f[r+c](S)

  return Z
}
```

Постійні значення для RC[i]:

```
RC[0]		0x0000000000000001		RC[12]		0x000000008000808B
RC[1]		0x0000000000008082		RC[13]		0x800000000000008B
RC[2]		0x800000000000808A		RC[14]		0x8000000000008089
RC[3]		0x8000000080008000		RC[15]		0x8000000000008003
RC[4]		0x000000000000808B		RC[16]		0x8000000000008002
RC[5]		0x0000000080000001		RC[17]		0x8000000000000080
RC[6]		0x8000000080008081		RC[18]		0x000000000000800A
RC[7]		0x8000000000008009		RC[19]		0x800000008000000A
RC[8]		0x000000000000008A		RC[20]		0x8000000080008081
RC[9]		0x0000000000000088		RC[21]		0x8000000000008080
RC[10]		0x0000000080008009		RC[22]		0x0000000080000001
RC[11] 		0x000000008000000A		RC[23]		0x8000000080008008
```
```
Pseudo-code of Keccak-f:

Keccak-f[b](A) {
    for i in 0…n-1   				// n=24
        A = Round[b](A, RC[i])
        return A
}

Round[b](A,RC) {
    # θ step
    C[x] = A[x,0] xor A[x,1] xor A[x,2] xor A[x,3] xor A[x,4],   	for x in 0…4
    D[x] = C[x-1] xor rot(C[x+1],1),                             	for x in 0…4
    A[x,y] = A[x,y] xor D[x],                           		    for (x,y) in (0…4,0…4)

    # ρ and π steps
    B[y,2*x+3*y] = rot(A[x,y], r[x,y]),                 		    for (x,y) in (0…4,0…4)

    # χ step
    A[x,y] = B[x,y] xor ((not B[x+1,y]) and B[x+2,y]),  		    for (x,y) in (0…4,0…4)

    # ι step
    A[0,0] = A[0,0] xor RC

    return A
}
```
Постійні значення r[x,y]:

![Постійні значення r[x,y]](/resources/img/practical-volume/6/1-keccak.png)

Сам текст стандарту та тестові вектори ви можете знайти за наступним посиланням:
https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf

✯ Спробуйте реалізувати алгоритм Keccak, використовуючи матеріал цього розділу

Михайло наголошує на наступній реалізації

🤖

```go
package main
import (
    "encoding/binary"
    "fmt"
)
// Константи Keccak
const (
    rate    = 136
    capacity = 256
    hashSize = capacity / 8
)
// Keccak-256 геш-функція
func Keccak256(input []byte) []byte {
    // Ініціалізація змінних
    state := [5][5]uint64{}
// Обробка повідомлення блоками
    for len(input) > 0 {
        var block []byte
        if len(input) >= rate/8 {
            block = input[0 : rate/8]
            input = input[rate/8:]
        } else {
            block = make([]byte, rate/8)
            copy(block, input)
            block[len(input)] = 0x01
            input = input[len(input):]
        }

// Додавання даних до стану
        addBlockToState(&state, block)

// Операції на стані
        keccakF(&state)
    }

// Виведення результату
    output := make([]byte, hashSize)
    binary.LittleEndian.PutUint64(output[0:8], state[0][0])
    binary.LittleEndian.PutUint64(output[8:16], state[1][0])
    binary.LittleEndian.PutUint64(output[16:24], state[2][0])
    binary.LittleEndian.PutUint64(output[24:32], state[3][0])
    binary.LittleEndian.PutUint64(output[32:40], state[4][0])

    return output
}
// Додавання блоку даних до стану
func addBlockToState(state *[5][5]uint64, block []byte) {
    for i := 0; i < len(block); i += 8 {
        index := i / 8
        state[index%5][index/5] ^= binary.LittleEndian.Uint64(block[i : i+8])
    }
}
// Операції над станом
func keccakF(state *[5][5]uint64) {
    // Ініціалізація змінних
    C := [5]uint64{}
    D := [5]uint64{}
    B := [5][5]uint64{}
// Крок 1
    for x := 0; x < 5; x++ {
        C[x] = state[x][0] ^ state[x][1] ^ state[x][2] ^ state[x][3] ^ state[x][4]
    }
// Крок 2
    for x := 0; x < 5; x++ {
        D[x] = C[(x+4)%5] ^ leftRotate(C[(x+1)%5], 1)
    }
// Крок 3
    for x := 0; x < 5; x++ {
        for y := 0; y < 5; y++ {
            B[x][y] = state[x][y] ^ D[x]
}
// Крок 4
var T [25]uint64
for x := 0; x < 5; x++ {
    for y := 0; y < 5; y++ {
        T[x+(y*5)] = B[x][y]
    }
}
// Крок 5
for x := 0; x < 5; x++ {
    for y := 0; y < 5; y++ {
        state[x][y] = T[x+(y*5)] ^ ((^T[((x+1)%5)+(y*5)]) & T[((x+2)%5)+(y*5)])
    }
}
}
// Циклічний зсув ліворуч
func leftRotate(n uint64, d uint) uint64 {
return (n << d) | (n >> (64 - d))
}
func main() {
// Приклад використання
data := []byte("The quick brown fox jumps over the lazy dog")
hash := Keccak256(data)
fmt.Printf("%x", hash)
}
```