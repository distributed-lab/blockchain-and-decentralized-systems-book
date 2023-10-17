# 2. Large Numbers

Through the examples and tasks provided in this section, it will become clear why it is necessary to use large keys.
We recommend familiarizing yourself with libraries that may be useful to you in advance:

- Java - <https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html>
- GoLang - <https://pkg.go.dev/math/big>
- Python (bignum type) - <https://peps.python.org/pep-0237/>

The task looks like:

1. Using one of the existing libraries (you can do it yourself, but it's better to use an existing one to save time), 
output the number of key options that can be specified by 8-, 16-, 32-, 64-, 128-, 256-, 512-, 1024-, 2048-, and 
4096-bit sequences.
   1. Example: If the key length is 16 bits, the key space is 65,536.
      1. Key space - the number of unique keys that can be specified by a number of a certain length.

   2. For each of the options, it is necessary to generate a random key value, which is in the range 0x00...0 to 0xFF...F 
depending on the selected key length.
   3. Write a function for brute-forcing values from the range to find the key. The goal of the function is to iterate 
through the key values from 0x00 ... 0 until a value equal to the previously generated key is found. The function 
should display the time in milliseconds that was spent finding the key.

An example implementation in GoLang for outputting the number of key variations for a given key length.
This code uses the math/big library to calculate the number of key parameters based on the key length. The Lsh method is 
used to shift a large integer number to the left, effectively multiplying it by 2 to the power of the given uint
value. The result is the number of unique keys that can be generated for each key length.

🤖

```go
package main
import (
	"fmt"
	"math/big"
)
const keyLengths = []int{8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096}
func main() {
	for _, length := range keyLengths {
		keySpace := big.NewInt(1)
		keySpace.Lsh(keySpace, uint(length))
		fmt.Printf("Key length: %d bits, Key space: %d\n", length, keySpace)
	}
}
Note that the brute-force method becomes very slow for larger key lengths, and it's not recommended to use it 
in practice due to security concerns. Brute-forcing is only for demonstration purposes in this case.

package main
import (
"crypto/rand"
"fmt"
"math/big"
"time"
)
const keyLengths = []int{8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096}
func main() {
for _, length := range keyLengths {
keySpace := big.NewInt(1)
keySpace.Lsh(keySpace, uint(length))
fmt.Printf("Key length: %d bits, Key space: %d\n", length, keySpace)

		key, _ := rand.Int(rand.Reader, keySpace)
		start := time.Now()
		bruteForce(key, keySpace)
		elapsed := time.Since(start)

		fmt.Printf("Time elapsed: %dms\n", elapsed.Milliseconds())
		fmt.Println("------------------------------")
	}
}
func bruteForce(key *big.Int, keySpace *big.Int) {
for i := big.NewInt(0); i.Cmp(keySpace) < 0; i.Add(i, big.NewInt(1)) {
if i.Cmp(key) == 0 {
fmt.Printf("Key found: %x\n", i)
return
}
}
}
```

✯ Try to implement the second and third parts of the task, which will provide an understanding of why to use large
keys.
