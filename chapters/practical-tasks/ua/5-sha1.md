# 5. Функція гешування SHA-1
Фактично, особливості функціонування SHA-1 полягає в циклічному перемішуванні та використанні основних бітових операцій 
над вхідними даними. Схема роботи одного циклу алгоритму SHA-1 виглядає так:

![Схема виконання одного циклу алгоритму SHA-1](/resources/img/practical-volume/5/1-sha1.png)

де Kt – константи;
Ft– змінна функція (змінюється кожні 20 циклів);
Wi – модифікований елемент вхідного повідомлення (4 байт);
<<< x – циклічний зсув вліво.

Сам текст стандарту та тестові вектори ви можете знайти за наступним посиланням:
https://csrc.nist.gov/csrc/media/publications/fips/180/2/archive/2002-08-01/documents/fips180-2.pdf

```
Pseudo-code:

function GetSHA1Hash (message) 
	begin
        h0 = 0x67452301
        h1 = 0xEFCDAB89
        h2 = 0x98BADCFE
        h3 = 0x10325476
        h4 = 0xC3D2E1F0 			//variables initialization	
        ml = message length in bits

        break message into 512-bit chunks (after message processing)
        for each chunk
            break chunk into sixteen 32-bit big-endian words w[i], 0 ≤ i ≤ 15
            for i from 16 to 79
                w[i] = (w[i-3] xor w[i-8] xor w[i-14] xor w[i-16]) leftrot 5
    
                a = h0
                b = h1
                c = h2
                d = h3
                e = h4


                for i from 0 to 79 {
                    if 0 ≤ i ≤ 19 then
                        f = (b and c) or ((not b) and d)
                        k = 0x5A827999
                    else if 20 ≤ i ≤ 39
                        f = b xor c xor d
                        k = 0x6ED9EBA1
                    else if 40 ≤ i ≤ 59
                        f = (b and c) or (b and d) or (c and d) 
                        k = 0x8F1BBCDC
                    else if 60 ≤ i ≤ 79
                        f = b xor c xor d
                        k = 0xCA62C1D6
                    temp = (a leftrot 5) + f + e + k + w[i]
                    e = d
                    d = c
                    c = b leftrotate 30
                    b = a
                    a = temp
                }
                h0 = h0 + a
                h1 = h1 + b 
                h2 = h2 + c
                h3 = h3 + d
                h4 = h4 + e
            }
        }
    hash = (h0 lshift 128) or (h1 lshift 96) or (h2 lshift 64) or (h3 lshift 32) or h4 	// concat
    end
```

Наш робот Михайло пропонує наступний варіант реалізації

🤖

```go

package main

import (
    "encoding/binary"
    "errors"
)

// Константи SHA-1
const (
    blockSize = 64
    digestSize = 20
)

// SHA-1 геш-функція
func SHA1(input []byte) ([]byte, error) {
    // Ініціалізація змінних
    var H [5]uint32
    H[0] = 0x67452301
    H[1] = 0xEFCDAB89
    H[2] = 0x98BADCFE
    H[3] = 0x10325476
    H[4] = 0xC3D2E1F0

    // Додавання padding до вхідного тексту
    input = append(input, 0x80)
    for len(input)%blockSize != 56 {
        input = append(input, 0x00)
    }

    // Додавання довжини вхідного тексту в бітах в кінці повідомлення
    length := uint64(len(input)) * 8
    padding := make([]byte, 8)
    binary.BigEndian.PutUint64(padding, length)
    input = append(input, padding...)

    // Розділення вхідного тексту на блоки
    blocks := splitToBlocks(input, blockSize)

    // Обробка кожного блоку
    for _, block := range blocks {
        // Розширення блоку
        W := make([]uint32, 80)
        for i := 0; i < 16; i++ {
            W[i] = binary.BigEndian.Uint32(block[i*4 : (i+1)*4])
        }
        for i := 16; i < 80; i++ {
            W[i] = leftRotate(W[i-3]^W[i-8]^W[i-14]^W[i-16], 1)
        }

        // Ініціалізація змінних
        a := H[0]
        b := H[1]
        c := H[2]
        d := H[3]
        e := H[4]

        // Обчислення хешу
        for i := 0; i < 80; i++ {
            var f uint32
            var k uint32

            if i < 20 {
                f = (b & c) | ((^b) & d)
                k = 0x5A827999
            } else if i < 40 {
                f = b ^ c ^ d
                k = 0x6ED9EBA1
            } else if i < 60 {
                f = (b & c) | (b & d) | (c & d)
                k = 0x8F1BBCDC
            } else {
                f = b ^ c ^ d
                k = 0xCA62C1D6
            }

            temp := leftRotate(a, 5) + f + e + k + W[i]
            e = d
            d = c
            c = leftRotate(b, 30)
            b = a
        a = temp
    }

    // Оновлення значень геш-функції
    H[0] += a
    H[1] += b
    H[2] += c
    H[3] += d
    H[4] += e
}

// Конвертація результату в байтовий масив
result := make([]byte, digestSize)
binary.BigEndian.PutUint32(result[0:4], H[0])
binary.BigEndian.PutUint32(result[4:8], H[1])
binary.BigEndian.PutUint32(result[8:12], H[2])
binary.BigEndian.PutUint32(result[12:16], H[3])
binary.BigEndian.PutUint32(result[16:20], H[4])

return result, nil}

// Розбиття вхідних даних на блоки
func splitToBlocks(input []byte, blockSize int) [][]byte {
var blocks [][]byte
for len(input) > 0 {
    if len(input) >= blockSize {
        blocks = append(blocks, input[0:blockSize])
        input = input[blockSize:]
    } else {
        block := make([]byte, blockSize)
        copy(block, input)
        blocks = append(blocks, block)
        input = input[len(input):]
    }
}

return blocks
}

// Циклічний зсув ліворуч
func leftRotate(n uint32, d uint) uint32 {
return (n << d) | (n >> (32 - d))
}
```

Ця реалізація генерує геш-значення тільки для повідомлень, які є кратними 64 байтам. Якщо вхідний текст не кратний 64 
байтам, потрібно додатково додати padding, який відповідає вимогам SHA-1.

Посилання на репозиторій з реалізацією: 
