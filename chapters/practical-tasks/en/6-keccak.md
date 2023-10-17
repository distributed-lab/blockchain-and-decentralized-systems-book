# 6. Keccak Algorithm
Keccak (SHA-3) is a variable-length hashing algorithm. The hash function is based on the construction of a
cryptographic sponge, in which data is first absorbed into the sponge. During this process, the output message **M**
undergoes multiple rounds of permutation **f**, and the result **Z** is "squeezed" from the sponge. During the "absorption"
phase, message blocks are summed modulo 2 with a subset of the state, after which the entire state is transformed
using the permutation function **f**. During the "squeezing" phase, output blocks are read from the same subset of the
state, which the permutation function **f**  has modified .

The basis of the compression function of the algorithm is the function **f**, which performs the shuffling of the
algorithm's internal state . The state **A** is represented as a 5x5 array, whose elements are 64-bit words initialized
with zero bits (i.e., the size of the state is 1600 bits). The function **f** performs 24 rounds, each of which produces
step-by-step transformations _**θ, ρ, π, χ, ι.**_

At the beginning of the transformation, padding is performed. Padding is necessary to bring the original
message to a string of fixed length. Then the "Absorb" function is performed. This function is designed to break
down the input (which has passed through the padding) string into components that will be sequentially used for
subsequent transformations:
- _**θ**_ transformation;
  _**ρ**_ and _**π**_ transformations;
  _**χ**_ transformation;
  **_ι_** transformation.
- After this, the Squeezing procedure is performed.
- This procedure is necessary for computing the final hash value.

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

Constant values for RC[i]:

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
Constant values r[x,y]:

![Constant values r[x,y]](/resources/img/practical-volume/6/1-keccak.png)

You can find the text of the standard and test vectors at the following link:
https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf

✯ Try implementing the Keccak algorithm using the material from this section.

Mykhailo emphasizes the following implementation.


🤖

```go
package main
import (
  "encoding/binary"
  "fmt"
)
// Keccak Constants
const (
  rate     = 136
  capacity = 256
  hashSize = capacity / 8
)
// Keccak-256 hash function
func Keccak256(input []byte) []byte {
  // Initialize variables
  state := [5][5]uint64{}
  // Process message in blocks
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
    // Add block to state
    addBlockToState(&state, block)
    // State operations
    keccakF(&state)
  }
  // Output result
  output := make([]byte, hashSize)
  binary.LittleEndian.PutUint64(output[0:8], state[0][0])
  binary.LittleEndian.PutUint64(output[8:16], state[1][0])
  binary.LittleEndian.PutUint64(output[16:24], state[2][0])
  binary.LittleEndian.PutUint64(output[24:32], state[3][0])
  binary.LittleEndian.PutUint64(output[32:40], state[4][0])
  return output
}
// Add block to state
func addBlockToState(state *[5][5]uint64, block []byte) {
  for i := 0; i < len(block); i += 8 {
    index := i / 8
    state[index%5][index/5] ^= binary.LittleEndian.Uint64(block[i : i+8])
  }
}
// State operations
func keccakF(state *[5][5]uint64) {
  // Initialize variables
  C := [5]uint64{}
  D := [5]uint64{}
  B := [5][5]uint64{}
  // Step 1
  for x := 0; x < 5; x++ {
    C[x] = state[x][0] ^ state[x][1] ^ state[x][2] ^ state[x][3] ^ state[x][4]
  }
  // Step 2
  for x := 0; x < 5; x++ {
    D[x] = C[(x+4)%5] ^ leftRotate(C[(x+1)%5], 1)
  }
  // Step 3
  for x := 0; x < 5; x++ {
    for y := 0; y < 5; y++ {
      B[x][y] = state[x][y] ^ D[x]
    }
  }
  // Step 4
  var T [25]uint64
  for x := 0; x < 5; x++ {
    for y := 0; y < 5; y++ {
      T[x+(y*5)] = B[x][y]
    }
  }
  // Step 5
  for x := 0; x < 5; x++ {
    for y := 0; y < 5; y++ {
      state[x][y] = T[x+(y*5)] ^ ((^T[((x+1)%5)+(y*5)]) & T[((x+2)%5)+(y*5)])
    }
  }
}
// Left rotate function
func leftRotate(n uint64, d uint) uint64 {
  return (n << d) | (n >> (64 - d))
}
func main() {
  // Example usage
  data := []byte("The quick brown fox jumps over the lazy dog")
  hash := Keccak256(data)
  fmt.Printf("%x", hash)
}
```
This implementation generates hashes with a length of 256 bits. If you need a longer hash, increase the capacity
parameter and change the value of the hashSize constant.