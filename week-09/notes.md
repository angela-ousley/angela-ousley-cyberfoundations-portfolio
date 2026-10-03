# Week 9 Notes - Vault Exchange Digital Trust

**Student Name:** Angela Ousley

**Week:** 9

Use your own words. Short notes and everyday examples are welcome. You do not need to memorize commands.

## From a Key to an Identity

Ivy has two keys with the same label. Why is the label not enough? How can checking a visitor badge help explain the problem?

```text
My explanation: The label isn't enough because anyone can name a public key to anything they want. A key can work, but the identity isn't proven just by looking at the key. You need evidence that links the key to an identity. Checking a visitor badge helps explain the problem because anyone can create a legit looking badge with a familiar logo and made up credentials. The badge will need to be inspected and verified that it is really from the company its claiming, the dates are valid, and the person on the badge is who they say they are.
```

## Certificate Fields

A certificate is a digital badge. Explain the name list (SAN), signing office (issuer), start/end dates, public key, signature method, and allowed job (purpose). Which field would you check to see whether the badge covers the right website?

```text
Field and its job: The SAN (subject alternative name) list shows the other names or domains the website goes by. The signing office (issuer) is the CA (certificate authority) that verifies the subject who is requesting a certificate. Start/end dates are when the certificate is valid. The subject's public key is on the certificate is used to bind the subject's name to the key. The signature method is the cryptographic recipe used by a Certificate Authority (CA) to sign the certificate. The Extended Key Usage (EKU) field shoes the allowed jobs the certificate can do. I'd check the SAN field to see if the badge covers the right website.
```

## Issuers and Accepted Trust

Who signed the badge? Who chooses whether to accept the top office? Explain why an office signing its own badge is not enough to make your browser trust it.

```text
My explanation: The certificate of authority (CA) signed the badge (certificate). The validator (browser) chooses whether to accept the top office by checking if the top office is in the trust store. An office signing its own badge is not enough to make your browser trust it because the client has to decide if the office can be trusted since no other trusted office has gone through the process of validating the office by verifying its identity.
```

## Key CSR and Certificate Roles

The CSR is a badge application. Explain the separate jobs of the service’s private key, application, office’s private key, and finished certificate. Who signs the application? Who signs the finished badge?

```text
My explanation: The service's private key is used to sign the badge application, the application is submitted in order to get verified by the CA and help prove the identity of the service, the office's private key signs the issued certificate to prove that the CA vouches for the service, and the finished certificate has information about the service and it's public key. The service (requestor) signs the application. The CA (issuer) signs the finished badge.
```

## TLS and Verification Evidence

Compare looking at a certificate, checking its saved file, and connecting to a running service. TLS sets up a protected connection. What did you observe in each activity?

```text
I looked at: The listener in terminal A and the client in terminal B to make sure it was connected.
I checked: I check to see if the subject name on the server's certificate matches the expected name.
I connected to: I connected to my loopback address 127.0.0.1 at port 8443 via TLS 1.3.
```

## Troubleshooting and Remediation

These words mean finding and fixing a problem. Record an actual message, what it meant, and your next step. Consider a wrong name, expired dates, wrong office, canceled certificate, or service that is not running. Label situations you only discussed; do not claim to have tested them.

```text
Message or situation: Wrong Name Error Message:
O = CyberFoundations Lab, CN = localhost
error 62 at 0 depth lookup: hostname mismatch
error certificates/service.cert.pem: verification failed
Exit status printed: Exit status: 2
What it means: It means the intended name that we are trying to verify doesn't match what is listed on the certificate.
What I would check or fix: I'd check the SAN list to see if the name is on there. If I was a was using the correct intended name to check against the certificate. I'd check to make sure I was using the right certificate in my verification process.
Which check I would repeat: If I discovered the name I was checking or the certificate I was using was incorrect, I'd go through verification steps again and check the correct name against the correct certificate.
```

## Questions I Still Have

```text
A word or step I want explained again: The TLS 1.3 main path was a bit confusing. I had to use outside resources to gain a better understanding of the process, but it's still not sticking with me very much.
```
