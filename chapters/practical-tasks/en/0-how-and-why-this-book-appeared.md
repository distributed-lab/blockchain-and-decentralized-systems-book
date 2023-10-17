# How and Why Did This Book Come About?

It is impossible to deny that in recent years, decentralized technologies have gained such momentum that even software developers who do not work with the entire stack of corresponding technologies somehow encounter individual cases of their use.
The authors of this book have been researching and summarizing the theoretical aspects of blockchain technology and decentralized technologies for many years, having vast practical experience in their application in commercial projects.

This book is a logical continuation of the three-volume textbook "[Blockchain and decentralized systems](https://books.distributedlab.com/en)", 
which covers various issues in  decentralized technologies, from the history of their emergence to the difficulties of implementation. The textbook focuses on how and why a particular technology is implemented in a certain way.
This practical book will show how to use, implement, and apply the theoretical aspects that were highlighted in the textbook. We also show in this book which critical points need attention  when building solutions that use decentralized technologies.

In this book, we have tried to collect practical cases with implementation options, from basic cryptographic algorithms to examples and tasks on implementing methods that highlight some concepts for deploying and implementing smart contracts.
The exercises in each section will help you apply what you have learned. All you need to start is basic programming skills.

## Who Is This Book For?

This book is for everyone. If it has reached you, it means you are already interested in decentralized technologies. We hope this practical guide will be useful to you, whether you are a student, an experienced developer, a blockchain enthusiast, or a university professor. 

Try to use the best practices for learning or creating your own laboratory workshops or technical solutions. All the examples and tasks on these pages were made and tested during interactive courses on "Blockchain and Decentralized Technologies" by Distributed Lab for a wide range of listeners.

## How to Read This Book?

We created  a practical guide in such a way that you read as little as possible and practiced more. We have developed and recorded only certain recommendations for the most effective mastery of technologies.
At the beginning of each section, there will be a small amount of theoretical material regarding the description of the problem that will be solved.
The next step will be a reference to the educational guide "Blockchain and Decentralized Systems" section or another source, in which the theoretical component, which we strongly recommend to familiarize yourself with, is described in detail.
Next, we will provide pseudocode of the algorithm or class diagram, separate parts of the implementation (we recommend not to approach them from the very beginning, but to try to solve the tasks independently), a link to GitHub, and examples of tasks that we propose to solve independently.
In addition, for some practical tasks, we will add examples of their solution by artificial intelligence and indicate certain shortcomings that were made in the presented implementation.

The main thing you should understand is that if you cannot solve the proposed task, it is not a reason for despair, but only a reason to refer again to the links mentioned above in the relevant section.

## Conventional Symbols

In order to simplify understanding between authors and readers, we will indicate that in addition to the main text, separate comments may be added in the following format:

> You can rely on these remarks as comments from the authors or an experienced developer.

-> You will be able to identify links to materials from the textbook "Blockchain and Decentralized Systems" in this way.

To illustrate parts of algorithms or methods using pseudocode, we will use the following format:

```
Pseudo-code:

m <-- length of key
for index, character in plaintext:
ciphertext[index] <-- (character + key[index % m]) % 26

return ciphertext
```

As for examples of specific implementation, you will find them in the following format:

```go
Programming language: GoLang go1.19

package stringutil_test
import (
    "fmt"
    "golang.org/x/example/stringutil"
)
func ExampleReverse() {
    fmt.Println(stringutil.Reverse("hello"))
    // Output: olleh
}
```

✯  – This symbol will indicate tasks that we suggest you solve on your own.

Typically, practical exercise books contain the best examples, practices, and provide "clean code". However, it is 
evident that when you are just starting to get acquainted with a certain technology, you cannot immediately make your 
code perfect for solving a specific task. We took this into account and want to provide examples with typical mistakes 
made by beginners and average students. As such a student, we chose [ChatGPT](https://openai.com/blog/chatgpt/) - a 
chatbot with artificial intelligence developed by OpenAI that can work in a dialog mode supporting natural language 
queries. ChatGPT is a large language model trained with supervised and reinforcement learning methods.

🤖 - This is the task solution formed by our friend Mikhail with artificial intelligence.

## Develop the Community with Us

We never stop at what has already been achieved because we strive to make each following day better than the previous 
one. The same goes for our books. We invite you to improve them together.

You can suggest topics that you think will be relevant in this book using 
this [form](https://forms.gle/EbNSp4NeTu7pSAYg6). We would be grateful if you write to us about any typos or other 
issues you find. Therefore, we would be thankful if you become a part of the authors' team for a few minutes :)

If you really want to thank the authors, please do so.
*Bitcoin donation address:* **1BooKnbm48Eabw3FdPgTSudt9u4YTWKBvf**

![Bitcoin Address](/resources/img/practical-volume/0/2_Bitcoin_address.png)

*Ethereum/BSC/Polygon/Arbitrum/ Avalanche/Optimism/Fantom donation address:* 
**0xB00CDEEc332c9bB877688cD15574f9CAc6cdE0f7**

![Ethereum Address](/resources/img/practical-volume/0/3_Ethereum_address.png)



