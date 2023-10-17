# 4. AES Cipher
The AES encryption algorithm defines numerous transformations that are performed on data stored in an array. AES 
operates on a 4x4 byte array called the state (in the simplest case).

![AES Operation Scheme](/resources/img/practical-volume/4/1-aes.png)

```
Pseudo-code:

function Encrypt(plaintext, key) {
    blocks := DivideIntoBlocks(plaintext);
    roundKeys = GetRoundKeys(key)
    for (block in blocks) {
    //first round
        AddRoundKey(roundKeys[0], block);
        //intermediate rounds
        for (8, 10 or 12 rounds) {
            SubBytes(block);
            ShiftRows(block);
            MixColumns(block);
            AddRoundKey(roundKeys[..], block);
        }
        //last round
        SubBytes(block);
        ShiftRows(block);
        AddRoundKey(roundKeys[numRounds - 1], block);
    }
    ciphertext := Reassemble(blocks);
    return ciphertext;
}
```

1. The function `DivideIntoBlocks` should divide the input text into blocks of 128 bits (16 bytes). Each block is 
represented as a matrix of 4x4 (an example is shown in the following figure).

| BA | AB | FF | 12 |
|:--:|:--:|:--:|:--:|
| 32 | 54 | 68 | F5 |
| 4E | AA | FF | 5F |
| 6D | EF | FE | 01 |

2. The function `GetRoundKeys` expands the key. Since AES assumes the use of a 128-bit key (in the simplest case),
this means that for data of greater length, a key sequence equal to the length of the encrypted message should be
generated. This is achieved through the use of the expansion function (a separate 128-bit encryption key is 
generated for each block being encrypted).


```
Pseudo-code:

function GetRoundKeys(byte key[4*Nk], word w[Nb*(Nr+1)], Nk) {
    begin
        word temp
        i = 0
        while (i < Nk)
            w[i] = word(key[4*i], key[4*i+1], key[4*i+2], key[4*i+3])
            i = i+1
        end while

        i = Nk

        while (i < Nb * (Nr+1)]
            temp = w[i-1]
            if (i mod Nk = 0)
                temp = SubWord(RotWord(temp)) xor Rcon[i/Nk]
            else if (Nk > 6 and i mod Nk = 4)
                temp = SubWord(temp)
            end if

            w[i] = w[i-Nk] xor temp
            i = i + 1
        end while
    end
}
```
3. The function `AddRoundKey` function performs the addition of a block with the RoundKey (XOR operation (⊕)).
4. The function `SubBytes` function is necessary for substitution: each byte in the block is replaced by the 
corresponding element from a fixed table (S-box).

![Substitution Table](/resources/img/practical-volume/4/3-substitution.png)

For convenience in implementation, you can copy the values for the table from the array below:
```
Sbox = array{
0x63, 0x7c, 0x77, 0x7b, 0xf2, 0x6b, 0x6f, 0xc5, 0x30, 0x01, 0x67, 0x2b, 0xfe, 0xd7, 0xab, 0x76,
0xca, 0x82, 0xc9, 0x7d, 0xfa, 0x59, 0x47, 0xf0, 0xad, 0xd4, 0xa2, 0xaf, 0x9c, 0xa4, 0x72, 0xc0,
0xb7, 0xfd, 0x93, 0x26, 0x36, 0x3f, 0xf7, 0xcc, 0x34, 0xa5, 0xe5, 0xf1, 0x71, 0xd8, 0x31, 0x15,
0x04, 0xc7, 0x23, 0xc3, 0x18, 0x96, 0x05, 0x9a, 0x07, 0x12, 0x80, 0xe2, 0xeb, 0x27, 0xb2, 0x75,
0x09, 0x83, 0x2c, 0x1a, 0x1b, 0x6e, 0x5a, 0xa0, 0x52, 0x3b, 0xd6, 0xb3, 0x29, 0xe3, 0x2f, 0x84,
0x53, 0xd1, 0x00, 0xed, 0x20, 0xfc, 0xb1, 0x5b, 0x6a, 0xcb, 0xbe, 0x39, 0x4a, 0x4c, 0x58, 0xcf,
0xd0, 0xef, 0xaa, 0xfb, 0x43, 0x4d, 0x33, 0x85, 0x45, 0xf9, 0x02, 0x7f, 0x50, 0x3c, 0x9f, 0xa8,
0x51, 0xa3, 0x40, 0x8f, 0x92, 0x9d, 0x38, 0xf5, 0xbc, 0xb6, 0xda, 0x21, 0x10, 0xff, 0xf3, 0xd2,
0xcd, 0x0c, 0x13, 0xec, 0x5f, 0x97, 0x44, 0x17, 0xc4, 0xa7, 0x7e, 0x3d, 0x64, 0x5d, 0x19, 0x73,
0x60, 0x81, 0x4f, 0xdc, 0x22, 0x2a, 0x90, 0x88, 0x46, 0xee, 0xb8, 0x14, 0xde, 0x5e, 0x0b, 0xdb,
0xe0, 0x32, 0x3a, 0x0a, 0x49, 0x06, 0x24, 0x5c, 0xc2, 0xd3, 0xac, 0x62, 0x91, 0x95, 0xe4, 0x79,
0xe7, 0xc8, 0x37, 0x6d, 0x8d, 0xd5, 0x4e, 0xa9, 0x6c, 0x56, 0xf4, 0xea, 0x65, 0x7a, 0xae, 0x08,
0xba, 0x78, 0x25, 0x2e, 0x1c, 0xa6, 0xb4, 0xc6, 0xe8, 0xdd, 0x74, 0x1f, 0x4b, 0xbd, 0x8b, 0x8a,
0x70, 0x3e, 0xb5, 0x66, 0x48, 0x03, 0xf6, 0x0e, 0x61, 0x35, 0x57, 0xb9, 0x86, 0xc1, 0x1d, 0x9e,
0xe1, 0xf8, 0x98, 0x11, 0x69, 0xd9, 0x8e, 0x94, 0x9b, 0x1e, 0x87, 0xe9, 0xce, 0x55, 0x28, 0xdf,
0x8c, 0xa1, 0x89, 0x0d, 0xbf, 0xe6, 0x42, 0x68, 0x41, 0x99, 0x2d, 0x0f, 0xb0, 0x54, 0xbb, 0x16
};
```

5. The `ShiftRows` function cyclically shifts bytes in each row of the block to the left by r bytes, depending on
the row number.

![Scheme of  the shift of rows in the block](/resources/img/practical-volume/4/4-shiftrow.png)

6. The`MixColumns` function involves multiplying each column of the block by a constant matrix as follows:


![MixColumns operation](/resources/img/practical-volume/4/5.mixcolumns.png)

The decryption is performed according to the algorithm:

```
Pseudo-code:

function InvCipher(byte in[4*Nb], byte out[4*Nb], word w[Nb*(Nr+1)])
    begin
        byte state[4,Nb]
        state = in

        AddRoundKey(state, w[Nr*Nb, (Nr+1)*Nb-1])

        for round = Nr-1 step -1 downto 1
            InvShiftRows(state) 
            InvSubBytes(state) 
            AddRoundKey(state, w[round*Nb, (round+1)*Nb-1])
            InvMixColumns(state) 
        end for

        InvShiftRows(state)
        InvSubBytes(state)
        AddRoundKey(state, w[0, Nb-1])
        out = state

    end
```
The functions `InvShiftRows`, `InvSubBytes`, and `InvMixColumns` are inverse - that is, they perform reverse
operations (4-6). 

✯ Try implementing the AES cipher using the material from this section.

You can find the standard and test vectors at the following link:: https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf

> Additionally, you can watch a video at the following link: https://www.youtube.com/watch?v=gP4PqVGudtg&ab_channel=Computerphile

The implementation approach chosen by our artificial intelligence robot looks like this.

🤖

```go
package main

import (
	"fmt"
)

// Global substitution table
var sbox = [...]byte{
    0x63, 0x7c, 0x77, 0x7b, 0xf2, 0x6b, 0x6f, 0xc5, 0x30, 0x01, 0x67, 0x2b, 0xfe, 0xd7, 0xab, 0x76,
    0xca, 0x82, 0xc9, 0x7d, 0xfa, 0x59, 0x47, 0xf0, 0xad, 0xd4, 0xa2, 0xaf, 0x9c, 0xa4, 0x72, 0xc0,
    0xb7, 0xfd, 0x93, 0x26, 0x36, 0x3f, 0xf7, 0xcc, 0x34, 0xa5, 0xe5, 0xf1, 0x71, 0xd8, 0x31, 0x15,
    0x04, 0xc7, 0x23, 0xc3, 0x18, 0x96, 0x05, 0x9a, 0x07, 0x12, 0x80, 0xe2, 0xeb, 0x27, 0xb2, 0x75,
    0x09, 0x83, 0x2c, 0x1a, 0x1b, 0x6e, 0x5a, 0xa0, 0x52, 0x3b, 0xd6, 0xb3, 0x29, 0xe3, 0x2f, 0x84,
    0x53, 0xd1, 0x00, 0xed, 0x20, 0xfc, 0xb1, 0x5b, 0x6a, 0xcb, 0xbe, 0x39, 0x4a, 0x4c, 0x58, 0xcf,
    0xd0, 0xef, 0xaa, 0xfb, 0x43, 0x4d, 0x33, 0x85, 0x45, 0xf9, 0x02, 0x7f, 0x50, 0x3c, 0x9f, 0xa8,
    0x51, 0xa3, 0x40, 0x8f, 0x92, 0x9d, 0x38, 0xf5, 0xbc, 0xb6, 0xda, 0x21, 0x10, 0xff, 0xf3, 0xd2,
    0xcd, 0x0c, 0x13, 0xec, 0x5f, 0x97, 0x44, 0x17, 0xc4, 0xa7, 0x7e, 0x3d, 0x64, 0x5d, 0x19, 0x73,
    0x60, 0x81, 0x4f, 0xdc, 0x22, 0x2a, 0x90, 0x88, 0x46, 0xee, 0xb8, 0x14, 0xde, 0x5e, 0x0b, 0xdb,
    0xe0, 0x32, 0x3a, 0x0a, 0x49, 0x06, 0x24, 0x5c, 0xc2, 0xd3, 0xac, 0x62, 0x91, 0x95, 0xe4, 0x79,
    0xe7, 0xc8, 0x37, 0x6d, 0x8d, 0xd5, 0x4e, 0xa9, 0x6c, 0x56, 0xf4, 0xea, 0x65, 0x7a, 0xae, 0x08,
    0xba, 0x78, 0x25, 0x2e, 0x1c, 0xa6, 0xb4, 0xc6, 0xe8, 0xdd, 0x74, 0x1f, 0x4b, 0xbd, 0x8b, 0x8a,
    0x70, 0x3e, 0xb5, 0x66, 0x48, 0x03, 0xf6, 0x0e, 0x61, 0x35, 0x57, 0xb9, 0x86, 0xc1, 0x1d, 0x9e,
    0xe1, 0xf8, 0x98, 0x11, 0x69, 0xd9, 0x8e, 0x94, 0x9b, 0x1e, 0x87, 0xe9, 0xce, 0x55, 0x28, 0xdf,
    0x8c, 0xa1, 0x89, 0x0d, 0xbf, 0xe6, 0x42, 0x68, 0x41, 0x99, 0x2d, 0x0f, 0xb0, 0x54, 0xbb, 0x16,
}

// Variables used for encryption and decryption
var key []byte = []byte("abcdefghijklmnop") // 16-byte key
var plainText []byte = []byte("testplaintext") // 16-byte text to be encrypted


func main() {
	cipherText := encrypt(plainText, key)
	fmt.Printf("Encrypted text: %x\n", cipherText)
	decryptedText := decrypt(cipherText, key)
	fmt.Printf("Decrypted text: %s\n", decryptedText)
}

// Encryption
func encrypt(plainText []byte, key []byte) []byte {
	// Divide input text into blocks of 16 bytes
	numBlocks := len(plainText) / 16
	if len(plainText)%16 != 0 {
		numBlocks++
	}
	// Fill text with zeros for the last block if the length is not a multiple of 16 bytes
	paddedText := make([]byte, numBlocks*16)
	copy(paddedText, plainText)
	// Divide key into blocks of 16 bytes
	keyBlocks := make([][]byte, 4)
	for i := 0; i < 4; i++ {
		keyBlocks[i] = key[i*4 : i*4+4]
	}
	// Encrypt each block of text
	cipherText := make([]byte, len(paddedText))
	for i := 0; i < numBlocks; i++ {
		block := paddedText[i*16 : i*16+16]
		cipherBlock := encryptBlock(block, keyBlocks)
		copy(cipherText[i*16:i*16+16], cipherBlock)
	}
	return cipherText
}

// Decryption
func decrypt(cipherText []byte, key []byte) []byte {
	// Divide input text into blocks of 16 bytes
	numBlocks := len(cipherText) / 16
	// Divide key into blocks of 16 bytes
	keyBlocks := make([][]byte, 4)
	for i := 0; i < 4; i++ {
		keyBlocks[i] = key[i*4 : i*4+4]
	}
	// Decrypt each block of text
	plainText := make([]byte, len(cipherText))
	for i := 0; i < numBlocks; i++ {
		block := cipherText[i*16 : i*16+16]
		plainBlock := decryptBlock(block, keyBlocks)
		copy(plainText[i*16:i*16+16], plainBlock)
	}
	// Remove zeros from the last block if they were added
	for i := 15; i > 0; i-- {
		if plainText[len(plainText)-i] == 0 {
			plainText = plainText[:len(plainText)-i]
		} else {
			break
		}
	}
	return plainText
}

// Function for encrypting one block of text
func encryptBlock(block []byte, keyBlocks [][]byte) []byte {
	// Step 1: add key
	addRoundKey(block, keyBlocks)
	// Steps 2-9
	for i := 1; i < 10; i++ {
		// Substitution of bytes from Sbox table
		subBytes(block)
		// Row shift
		shiftRows(block)
		// Column mixing
		mixColumns(block)
		// Adding round key
		addRoundKey(block, keyBlocks[i])
	}
	// Step 10: byte substitution and adding round key
	subBytes(block)
	shiftRows(block)
	addRoundKey(block, keyBlocks[10])
	return block
}
// Function for decrypting one block of text
func decryptBlock(block []byte, keyBlocks [][]byte) []byte {
	// Step 1: add key
	addRoundKey(block, keyBlocks[10])
	// Steps 2-9 (in reverse order)
	for i := 9; i > 0; i-- {
		// Reverse substitution of bytes from Sbox table
		invSubBytes(block)
		// Reverse row shift
		invShiftRows(block)
		// Reverse column mixing
		invMixColumns(block)
		// Adding round key
		addRoundKey(block, keyBlocks[i])
	}
	// Step 10: reverse byte substitution and adding key
	invSubBytes(block)
	invShiftRows(block)
	addRoundKey(block, keyBlocks[0])
	return block
}
// Substitution of bytes from Sbox table
func subBytes(block []byte) {
	for i := 0; i < 16; i++ {
		block[i] = Sbox[block[i]]
	}
}
// Reverse substitution of bytes from Sbox table
func invSubBytes(block []byte) {
	for i := 0; i < 16; i++ {
		block[i] = invSbox[block[i]]
	}
}

// Shift rows to the left
func shiftRows(block []byte) {
	// Shift 2nd row by 1 byte to the left
	block[1], block[5], block[9], block[13] = block[5], block[9], block[13], block[1]
	// Shift 3rd row by 2 bytes to the left
	block[2], block[6], block[10], block[14] = block[10], block[14], block[2], block[6]
	block[2], block[6], block[10], block[14] = block[10], block[14], block[2], block[6]
	// Shift 4th row by 3 bytes to the left
	block[3], block[7], block[11], block[15] =
		block[15], block[3], block[7], block[11]
}
// Reverse shift rows to the right
func invShiftRows(block []byte) {
	// Shift 2nd row by 1 byte to the right
	block[1], block[5], block[9], block[13] = block[13], block[1], block[5], block[9]
	// Shift 3rd row by 2 bytes to the right
	block[2], block[6], block[10], block[14] = block[10], block[14], block[2], block[6]
	block[2], block[6], block[10], block[14] = block[10], block[14], block[2], block[6]
	// Shift 4th row by 3 bytes to the right
	block[3], block[7], block[11], block[15] = block[11], block[15], block[3], block[7]
}
// Mix columns
func mixColumns(block []byte) {
	for i := 0; i < 4; i++ {
		// Copy column to a separate array
		column := make([]byte, 4)
		copy(column, block[i4:i4+4])
		// Mix column
		block[i*4] = multiply(0x02, column[0]) ^ multiply(0x03, column[1]) ^ column[2] ^ column[3]
		block[i*4+1] = column[0] ^ multiply(0x02, column[1]) ^ multiply(0x03, column[2]) ^ column[3]
		block[i*4+2] = column[0] ^ column[1] ^ multiply(0x02, column[2]) ^ multiply(0x03, column[3])
		block[i*4+3] = multiply(0x03, column[0]) ^ column[1] ^ column[2] ^ multiply(0x02, column[3])
	}
}
// Reverse mix columns
func invMixColumns(block []byte) {
	for i := 0; i < 4; i++ {
		// Copy column to a separate array
		column := make([]byte, 4)
		copy(column, block[i4:i4+4])
		// Reverse mix column
		block[i*4] = multiply(0x0e, column[0]) ^ multiply(0x0b, column[1]) ^ multiply(0x0d, column[2]) ^ multiply(0x09, column[3])
		block[i*4+1] = multiply(0x09, column[0]) ^ multiply(0x0e, column[1]) ^ multiply(0x0b, column[2]) ^ multiply(0x0d, column[3])
		block[i*4+2] = multiply(0x0d, column[0]) ^ multiply(0x09, column[1]) ^ multiply(0x0e, column[2]) ^ multiply(0x0b, column[3])
		block[i*4+3] = multiply(0x0b, column[0]) ^ multiply(0x0d, column[1]) ^ multiply(0x09, column[2]) ^ multiply(0x0e, column[3])
	}
}
// Add round key
func addRoundKey(block []byte, keyBlock []byte) {
	for i := 0; i < 16; i++ {
		block[i] ^= keyBlock[i]
	}
}

// Galois field multiplication function
func multiply(a byte, b byte) byte {
	// Binary representation of b
	binaryB := make([]byte, 8)
	for i := 0; i < 8; i++ {
		binaryB[i] = (b >> uint(i)) & 1
	}
	// Multiplication of a and b
	result := byte(0)
	for i := 0; i < 8; i++ {
		if binaryB[i] == 1 {
			result ^= a << uint(i)
		}
	}
	// Reduction over the Galois field
	for i := 14; i >= 8; i-- {
		if (result >> uint(i)) & 1 == 1 {
			result ^= 0x11b << uint(i-8)
		}
	}
	return result
}
// Main AES encryption function
func EncryptAES(input []byte, key []byte) ([]byte, error) {
	// Check input text and key length
	if len(input)%16 != 0 {
		return nil, fmt.Errorf("input length must be a multiple of 16")
	}
	if len(key) != 16 {
		return nil, fmt.Errorf("key length must be 16")
	}
	// Split key into blocks and obtain round keys
	keyBlocks := splitToBlocks(key, 16)
	roundKeys := make([][]byte, 11)
	roundKeys[0] = keyBlocks[0]
	for i := 1; i < 11; i++ {
		roundKeys[i] = generateRoundKey(roundKeys[i-1], byte(i))
	}
	// Encrypt input text blocks
	encryptedBlocks := make([][]byte, len(input)/16)
	for i := 0; i < len(input)/16; i++ {
		block := input[i*16 : i*16+16]
		// Initial round key addition
		addRoundKey(block, roundKeys[0])
		// Encryption rounds 1-9
		for j := 1; j <= 9; j++ {
			subBytes(block)
			shiftRows(block)
			mixColumns(block)
			addRoundKey(block, roundKeys[j])
		}
		// Encryption round 10
		subBytes(block)
		shiftRows(block)
		addRoundKey(block, roundKeys[10])
		// Add encrypted block to array
		encryptedBlocks[i] = block
	}
	// Merge encrypted blocks into one array and return result
	encrypted := make([]byte, len(input))
	for i, block := range encryptedBlocks {
		copy(encrypted[i*16:i*16+16], block)
	}
	return encrypted, nil
}
// Main AES decryption function
func DecryptAES(input []byte, key []byte) ([]byte, error) {
	// Check input text and key length
	if len(input)%16 != 0 {
		return nil, fmt.Errorf("input length must be a multiple of 16")
	}
	if len(key) != 16 {
		return nil, fmt.Errorf("key length must be 16")
	}
	// Split key into blocks and obtain round keys
	keyBlocks := splitToBlocks(key, 16)
	roundKeys := make([][]byte, 11)
	roundKeys[0] = keyBlocks[0]
	for i := 1; i < 11; i++ {
		roundKeys[i] = generateRoundKey(roundKeys[i-1], byte(i))
	}
	// Decrypt input text blocks
	decryptedBlocks := make([][]byte, len(input)/16)
	for i := 0; i < len(input)/16; i++ {
		block := input[i16 : i16+16]
		// Initial round key subtraction
		addRoundKey(block, roundKeys[10])
		// Decryption rounds 9-1
		for j := 9; j >= 1; j-- {
			inverseShiftRows(block)
			inverseSubBytes(block)
			addRoundKey(block, roundKeys[j])
			inverseMixColumns(block)
		}
	}


	// Round 0 Decryption
	inverseShiftRows(block)
	inverseSubBytes(block)
	addRoundKey(block, roundKeys[0])
	// Add decrypted block to array
	decryptedBlocks[i] = block
}
// Combine decrypted blocks into one array and return result
decrypted := make([]byte, len(input))
for i, block := range decryptedBlocks {
copy(decrypted[i*16:i*16+16], block)
}
return decrypted, nil
}
// Function for splitting an array into blocks of a specified length
func splitToBlocks(input []byte, blockSize int) [][]byte {
	blocks := make([][]byte, len(input)/blockSize)
	for i := 0; i < len(input)/blockSize; i++ {
		blocks[i] = input[iblockSize : iblockSize+blockSize]
	}
	return blocks
}
// Function for generating a round key
func generateRoundKey(keyBlock []byte, round byte) []byte {
	// Byte rotation in column to the right
	rotated := make([]byte, 4)
	copy(rotated, keyBlock[1:4])
	rotated[3] = keyBlock[0]
	// Substitution of bytes according to the substitution table
	for i := 0; i < 4; i++ {
		rotated[i] = Sbox[rotated[i]]
	}
	// Adding the round constant to the first byte
	rotated[0] ^= Rcon[round]
	// Combination with the previous round key
	result := make([]byte, 16)
	copy(result[0:4], xor(keyBlock[0:4], rotated))
	for i := 1; i < 4; i++ {
		copy(result[i*4:(i+1)*4], xor(result[(i-1)*4:i*4], keyBlock[i*4:(i+1)*4]))
	}
	return result
}
// Function for bitwise XOR of two arrays
func xor(a []byte, b []byte) []byte {
	result := make([]byte, len(a))
	for i := 0; i < len(a); i++ {
		result[i] = a[i] ^ b[i]
	}
	return result
}
```

This is a complete implementation of the AES cipher using the specified Sbox substitution table. Use this code
for encrypting and decrypting data according to the AES algorithm. Note that this implementation only supports a
key length of 16 bytes and an input text length that is a multiple of 16 bytes. If you need to support other key
or input text sizes, you will need to make the appropriate changes.
