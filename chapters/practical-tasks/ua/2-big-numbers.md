# 2. Великі числа
На прикладах та завданнях, які будуть наведені в цьому розділі, стане зрозуміло, навіщо використовувати ключі великого 
розміру.

Заздалегідь рекомендуємо ознайомитися з бібліотеками, які можуть стати вам у нагоді:

Java - https://docs.oracle.com/javase/7/docs/api/java/math/BigInteger.html

GoLang -	https://pkg.go.dev/math/big 	

Python (bignum type) - https://peps.python.org/pep-0237/

Завдання має вигляд:
1. Використовуючи одну з існуючих бібліотек (можете зробити самостійно, але краще все-таки використовувати існуючу для 
економії часу) вивести кількість варіантів ключів, які можна задати 8-, 16-, 32-, 64-, 128-, 256-, 512- , 1024-, 2048-, 
4096-бітною послідовністю. 
   1. Приклад: Якщо довжина ключа дорівнює 16 бітів – то простір можливих ключів становить 65 536. 
      1. Простір ключів – кількість унікальних ключів які можуть бути задані числом певної довжини.

З реалізацію цієї частини ви можете ознайомитися в нашому репозиторії

Посилання на репозиторій з реалізацією: https://GitHub/distributedlab

2. Для кожного з варіантів необхідно згенерувати випадкове значення ключа, яке знаходиться в діапазоні 0x00…0 до 0xFF…F 
залежно від обраної довжини ключа. 
3. Написати функцію для брутфорсу значень з діапазону з метою знаходження ключа. Мета функції перебирати значення ключа 
від 0x00 ... 0 до тих пір, поки буде не знайдено значення, рівне попередньо згенерованого ключового. Функція повинна 
виводити кількість часу в мілісекундах, яка була витрачена для знаходження ключа.

Приклад реалізації від  для виведення кількості варіантів ключів для заданої довжини ключа на мові GoLang.

Цей код використовує бібліотеку math/big для обчислення кількості параметрів ключа на основі довжини ключа. Метод Lsh 
використовується для зсуву вліво великого цілого числа, яке фактично множить його на 2 у степені даного значення uint. 
Результатом є кількість унікальних ключів, які можна згенерувати для кожної довжини ключа.

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

Note that the brute-force method becomes very slow for larger key lengths, and it's not recommended to use it in practice due to security concerns. Brute-forcing is only for demonstration purposes in this case.
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
✯ Спробуйте реалізувати другу та третю частину завдання, що надасть розуміння того, навіщо використовувати ключі 
великого розміру.

Посилання на репозиторій з прикладом вирішеного завдання: 

