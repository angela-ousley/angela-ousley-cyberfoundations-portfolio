# Week 8 Notes — Practical Cryptography

**Student Name:** Angela Ousley

**Date:** September 13, 2026

## Vocabulary in My Own Words

- Plaintext: readable data in its unencrypted form
- Ciphertext: scrambled unreadable data that’s encrypted
- Encryption: a mathematical transformation of data that makes it unreadable without the key
- Hash / digest: a value given after data goes through a hash algorithm
- Public key: a shared key used to verify and decrypt data
- Private key: a key that’s kept secret by the owner to encrypt data and apply a digital signature
- Digital signature: a hash value that has been signed using a private key
- `authorized_keys`: a file on a server that holds public keys

## Command-to-Purpose Map

| Command | What it demonstrated |
| --- | --- |
| `openssl enc` | Encrypting a file with a password |
| `sha256sum` | Using the SHA 256 algorithm to create a hash of the file |
| `ssh-keygen` | Generates a public and private key pair |
| `openssl dgst` | Used with the -sign and -verify flags to open the digest (hash value) and sign it and verify it |
| `ssh ... -o PasswordAuthentication=no` | Removed the option to authenticate SSH via password |

## Safety Rules I Must Remember

1. Never submit or post .pem files, passwords, or passphrases
2. Never share or post your Bastion URL
3. Never share your private key!

## Question for the Instructor
None at the momement
