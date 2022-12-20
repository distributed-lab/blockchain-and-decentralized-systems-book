# 3 Cryptography and keys management
## 3.1 Introduction to cryptography
In this subsection, without going into a deep dive, we are going to get acquainted with cryptography as a science, its 
tasks, and basic mechanisms. We will consider the specific cryptographic mechanisms and functions applied in the Bitcoin 
protocol. Also, we will pay attention to the working features of a digital signature and the digital keys used for 
operating with it; these aspects are primarily of a practical nature.

*Cryptography is the science of information protection using mathematical methods*. Its role in the development of 
information systems is hard to overestimate. Bitcoin became the world's first payment system where the main processes 
and operations are cryptographically protected.

You will have an opportunity to learn the basic principles of information security and consider the most important 
cryptographic algorithms related to the Bitcoin protocol.

### Principles of cryptographic information security
The development of cryptography began in ancient times. Its history dates back more than four thousand years. Initially, 
cryptography solved only one problem—ensuring confidentiality when transferring and storing data. To solve this problem 
then, such mechanisms as text shuffling, mixing of text symbols, use of alternative alphabets, etc. were used.

At present, cryptography uses a mathematical basis. It involves special transformations which are defined in branches of 
mathematics such as number and information theories, group theory, ring theory, field theory, as well as in some others. 
This kind of mathematics is not well-known to most people since it is not taught in most educational institutions.

Modern cryptography includes symmetric and asymmetric encryption schemes, digital signature schemes, data hashing 
schemes, key management methods, zero-knowledge proof schemes, post-quantum cryptography, etc.

> **Basic security services**
>> * Ensuring data ***confidentiality***
>> * Ensuring data ***integrity***
>> * Ensuring data ***accessibility***
>> * Ensuring data ***authenticity***

*Confidentiality* implies that unauthorized persons cannot access the semantic content of a document that is stored or 
transmitted Most often this service is provided via encryption (symmetric or asymmetric).
Integrity implies that unauthorized persons cannot modify data unnoticeably. The task of verifying the integrity of data read from a carrier or received from a network is typically solved via hashing and checksum verification.
Accessibility implies that the parties having the right to access some information will definitely gain access to it.
Authenticity implies the ability to prove that data have definitely been received from a specific author. The task of ensuring the authenticity of data is typically solved using a digital signature. We give its simplified definition below.
