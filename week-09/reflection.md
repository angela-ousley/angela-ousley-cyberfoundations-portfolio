# Week 9 Reflection - Vault Exchange Digital Trust

**Student Name:** Angela Ousley

**Week:** 9

1. What clicked for you this week?

```text
What fields I should look at when verifying a certificate. The trust chain. What a CSR is. What a CA does.
```

2. What's still confusing?

```text
TLS 1.3 path.
```

3. How does this week's material connect to a cybersecurity career path you're interested in?

```text
Using the CLI to create asymmetric keys is something I may have to do in a future role. Testing to see if a server is listening is a good tool to have for network troubleshooting. 
```

4. One thing you would tell a friend just starting this course:

```text
Sometimes outside resources are needed to increase your understanding of the material.
```

## Professional Growth Check

- [x] I can explain why a working key’s label does not prove who owns it.

- [x] I can read the digital badge and describe the list of signing offices I actually saw.

- [x] I can explain the service’s key, its badge application, the office’s key, and the finished badge.

- [x] I can explain why correct information passed and why an intentionally wrong input was refused.

- [x] I can tell the difference between a service not answering and its badge failing a check.

- [x] I can share useful screenshots while keeping private keys and passwords secret.

## Portfolio Deliverable 3 Reflection

Write 5–7 sentences. What can a correctly checked digital badge tell you about the service? What can it NOT promise? Describe one problem using the message you actually saw, and explain what you checked next. Compare the real website with your practice service. You may start with “I used to think…”, “My check showed…”, and “I now know…”.

```text
A correctly checked digital badge tells me that the service has been verified by the CA, the validity dates, what the cert is allowed to do, the subject alternative names, and other fields. It can't promise that the website itself is safe. One problem with the message I saw about a name mismatch is that the intended name didn't match the name on the certificate. I checked to make sure I was using the right intended name and checking it against the correct certificate. Now I know that a name mismatch through a check on my browser will show a warning to enter the website, while the command line will show an error message saying that the hostname doesn't match and the verification failed.
```
