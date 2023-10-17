# 1. Big Endian and Little Endian Byte Order

The data is stored in separate cells, each storing a byte (8 bits). It is possible to store a single value that exceeds 
8 bits as a sequence of bytes. The important thing is the sequence in which we store the bytes (depending on the system
being used). Sometimes, storing the least significant byte in the first memory cell and the most significant byte in the
last one is used, and sometimes it is the opposite. In this case, it is important how the hexadecimal (hex) value is
converted to an unsigned integer value.

![Data allocation in the memory](/resources/img/practical-volume/1/1-bytes.png)

To understand how conversions are made, we suggest writing a library that allows you to:
* Convert HEX value to Little Endian value
* Convert HEX value to Big Endian value
* Convert Little Endian value to HEX value
* Convert Big Endian value to HEX value

* Test vectors:

```
Vector 1:
    Value: 0xff00000000000000000000000000000000000000000000000000000000000000
    Number of bytes: 32
    Little-endian: 255
    Big-endian: 115339776388732929035197660848497720713218148788040405586178452820382218977280

Vector 2:
    Value: 0xaaaa000000000000000000000000000000000000000000000000000000000000
    Number of bytes: 32
    Little-endian:	43690
    Big-endian:	77193548260167611359494267807458109956502771454495792280332974934474558013440

Vector 3:
    Value: 0xFFFFFFFF
    Number of bytes: 4
    Little-endian:	4294967295
    Big-endian:	4294967295

Vector 4:
    Value: 0xF000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    Number of bytes: 512
    Little-endian:	240
    Big-endian:	979114576324830475023518166296835358668716483481922294110218890578706788723335115795775136189060210944584475044786808910613350098299181506809283832360654948074334665509728123444088990750984735919776315636114949587227798911935355699067813766573049953903257414411690972566828795693861196044813729172123152193769005290826676049325224028303369631812105737593272002471587527915367835952474124875982077070337970837392460768423348044782340688207323630599527945406427226264695390995320400314062984891593411332752703846859640346323687201762934524222363836094053204269986087043470117703336873406636573235808683444836432453459818599293667760149123595668832133083221407128310342064668595954073131257995767262426534143159642539179485013975461689493733866106312135829807129162654188209922755829012304582671671519678313609748646814745057724363462189490278183457296449014163077506949636570237334109910914728582640301294341605533983878368789071427913184794906223657920124153256147359625549743656058746335124502376663710766611046750739680547042183503568549468592703882095207981161012224965829605768300297615939788368703353944514111011011184191740295491255291545096680705534063721012625490368756140460791685877738232879406346334603566914069127957053440
```

✯ Try to implement the conversion of HEX value to Big Endian value, conversion of Little Endian value to HEX value,
conversion of BIG Endian value to HEX value, using the material and test vectors provided in this section. 

Our AI student suggests that you familiarize yourself with his implementation of the task of converting HEX value
to Little Endian value.

🤖 This implementation takes a hex string as input, converts it to Little Endian byte representation, and then converts it 
to a uint64 value using the `littleEndianToUint64()` function. The `reverseBytes()` function is used to reverse the 
order of bytes.

```go
//The function of converting a HEX string into a binary array:
func hexToBytes(str string) []byte {
    b, _ := hex.DecodeString(str)
    return b
}
//The function of converting a binary array into Little Endian format:
func littleEndian(b []byte) []byte {
    var le []byte
    for i := len(b) - 1; i >= 0; i-- {
        le = append(le, b[i])
    }
    return le
}
//Combining functions into one that accepts a HEX string and returns its representation in //Little Endian format:
func hexToLittleEndian(str string) []byte {
    b := hexToBytes(str)
    le := littleEndian(b)
    return le
}
//Adding test vectors and checking the correct operation of the function:
func main() {
    // Vector 1
    str1 := "ff00000000000000000000000000000000000000000000000000000000000000"
    le1 := hexToLittleEndian(str1)
    fmt.Printf("Value: %s\n", str1)
    fmt.Printf("Number of bytes: %d\n", len(le1))
    fmt.Printf("Little-endian: %d\n", new(big.Int).SetBytes(le1).Int64())
    fmt.Printf("Big-endian: %s\n\n", new(big.Int).SetBytes(b).String())

    // Vector 2
    str2 := "aaaa000000000000000000000000000000000000000000000000000000000000"
    le2 := hexToLittleEndian(str2)
    fmt.Printf("Value: %s\n", str2)
    fmt.Printf("Number of bytes: %d\n", len(le2))
    fmt.Printf("Little-endian: %d\n", new(big.Int).SetBytes(le2).Int64())
    fmt.Printf("Big-endian: %s\n\n", new(big.Int).SetBytes(b).String())

    // Vector 3
    str3 := "ffffffff"
    le3 := hexToLittleEndian(str3)
    fmt.Printf("Value: %s\n", str3)
    fmt.Printf("Number of bytes: %d\n", len(le3))
    fmt.Printf("Little-endian: %d\n", new(big.Int).SetBytes(le3).Int64())
    fmt.Printf("Big-endian: %s\n\n", new(big.Int).SetBytes(b).String())

    // Vector 4
    str4 := strings.Repeat("f0", 256)
    le4 := hexToLittleEndian(str4)
    fmt.Printf("Value: %s\n", str4)
    fmt.Printf("Number of bytes: %d\n", len(le4))
    fmt.Printf("Little-endian: %d\n", new(big.Int).SetBytes(le4).Int64())

//Here's the completed implementation for the Little Endian conversion and the test vectors:
package main
import (
	"encoding/hex"
	"fmt"
)
func hexToLittleEndian(hexStr string) []byte {
	hexBytes, _ := hex.DecodeString(hexStr)

	reversed := make([]byte, len(hexBytes))
	for i := range hexBytes {
		reversed[len(hexBytes)-1-i] = hexBytes[i]
	}
	return reversed
}
func littleEndianToUint64(littleEndian []byte) uint64 {
	var value uint64
	for i, b := range littleEndian {
		value |= uint64(b) << uint(i*8)
	}
	return value
}
func main() {
	testVectors := []struct {
		hexValue        string
		littleEndianHex string
	}{
		{
			hexValue:        "ff00000000000000000000000000000000000000000000000000000000000000",
			littleEndianHex: "ff00000000000000000000000000000000000000000000000000000000000000",
		},
		{
			hexValue:        "aaaa000000000000000000000000000000000000000000000000000000000000",
			littleEndianHex: "aaaa000000000000000000000000000000000000000000000000000000000000",
		},
		{
			hexValue:        "FFFFFFFF",
			littleEndianHex: "FFFFFFFF",
		},
		{
			hexValue:        "F000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
			littleEndianHex: "F0",
		},
	}

	for _, vector := range testVectors {
		littleEndian := hexToLittleEndian(vector.hexValue)
		uint64Value := littleEndianToUint64(littleEndian)
		fmt.Printf("Value: %s\nLittle-endian: %d\nBig-endian: %d\n", vector.hexValue, uint64Value, littleEndianToUint64(reverseBytes(littleEndian)))
	}
}
func reverseBytes(input []byte) []byte {
	for i := 0; i < len(input)/2; i++ {
		j := len(input) - 1 - i
		input[i], input[j] = input[j], input[i]
	}
	return input
}
```
