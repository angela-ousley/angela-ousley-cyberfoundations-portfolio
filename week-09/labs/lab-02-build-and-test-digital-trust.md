# Week 9 Lab 02 - Build and Test Digital Trust

**Student Name:** Angela Ousley

**Date Completed:** September 27, 2026

**Module:** 3 - Practical Cryptography | **Week:** 9  
**Submission Path:** `week-09/labs/lab-02-build-and-test-digital-trust.md`

> ## Vault Exchange Trust and Key Safety Rule
> Use your assigned Ubuntu training computer (VM) as `analyst`. Keep Week 6–8 files and your SSH login keys unchanged. Do not run `sudo` (administrator commands), change network rules, add your pretend badge office to the computer or browser’s accepted list, or make the practice server available to other computers. All new work stays inside a fresh practice folder under `~/cloud-heights/week9-digital-trust/`.
>
> **Evidence safety:** Never display, upload, or commit private-key contents. Publish selected worksheet text and reviewed screenshots only. Do not upload the whole VM workspace or run `git add .` from it. Crop credentials, access URLs, and account information.

---

## Mission

Ivy needs a digital badge for a practice service running on her training computer. You will help her apply for the badge, issue it from a pretend badge office, and check it.

The digital badge is a **certificate**. The issuing office is a **certificate authority**, shortened to **CA**. You will play both roles for this exercise. Your pretend office will not become trusted by public websites or other people’s browsers.

You will also try a wrong name and a wrong office on purpose. A check refusing the wrong information is the result we want. Finally, you will compare your practice service with the real website from Lab 01.

The badge application is called a **certificate signing request (CSR)**. Signing an application shows that its maker used the matching secret key. It does not prove they have permission to get a badge for someone else’s website. A real issuing office must check that permission before issuing a certificate.

## What You Already Know

Think back to these ideas; you do not need to memorize the technical names:

- A **service** is a program waiting to help another program, like a reception desk waiting for visitors.
- A **VM (virtual machine)** is your assigned training computer, accessed through Cloud Heights.
- A **terminal** is a window where you type instructions, called **commands**. **Bash** reads and runs those instructions.
- **OpenSSL** is the tool we will use to make keys and check certificates.
- A **key pair** has a secret private key and a shareable public key. Never share the private key.
- **TLS** is the method programs use to set up a protected connection. It checks identity and arranges keys to protect the messages.

Looking at a badge, checking a badge, and successfully using it at a door are different activities. We will collect evidence for each one.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Assigned Cloud Heights Ubuntu VM, account `analyst`, Bash shell |
| Tools | OpenSSL 3.x, core utilities, `ss`, and two terminal sessions |
| Working root | `~/cloud-heights/week9-digital-trust/` |
| Separate practice area | Fresh `practice-*` folder; no preloaded certificate fixture assumed |
| Practice connection address | `127.0.0.1:8443`, expected DNS name `localhost` |
| Time | 90-120 minutes; work in Parts A-D with pauses |
| Submission | Selected evidence only; private keys stay on the VM |

- [x] I completed Lab 01 or have a dated public-site observation ready.

- [x] I am on my assigned VM and can open a second terminal session to that same VM.

- [x] My account is `analyst` and I can write to the Week 9 workspace.

- [x] I know that an intentional negative test is successful learning evidence when it rejects for the intended reason.

### Cloud Heights Idle Stop

Respond to the Portal's idle warning while actively working. A stopped/deallocated VM is not deleted; restart it from My Lab Environment. Saved files remain, but the TLS listener must be started again after a restart. Reopen your recorded practice path instead of regenerating keys over existing files.

### Before You Type Commands

1. Work in your assigned VM, using the account named `analyst`. Do not paste these commands into your personal computer’s terminal.
2. Copy one entire **bash** box at a time, paste it into the terminal, and press Enter if the last line has not run. Do not copy the box borders or the word `bash`.
3. Wait for your usual command prompt to return before pasting the next box. A **prompt** is the terminal’s “ready for your next command” line. Step 10 is the exception: the service keeps running there.
4. Long commands may wrap onto another screen line. Copy the whole command. You do not need to memorize the options beginning with `-`.
5. Text in **text** boxes is for your worksheet answers, not the terminal. Replace the blank prompts with your own observations.
6. If the result differs from the expected result, stop at that step and show your instructor the command and error. Do not remove checking options to make an error disappear.

A **path** is a file or folder’s address. `~` means your home folder. `cd` means “move into this folder”; `pwd` prints the folder you are currently in. A value such as `W9_RUN` is a named storage box the command uses to remember a path. Keep the commands exactly as written.

## Predict First

Make a best guess for each test. “Expected name” means the name we want the badge to cover. A “listener” is a running program waiting for a connection. Port `8443` is the numbered door this program uses. Explain each prediction in 2–3 sentences; it is okay to be unsure before trying it.

| Test | Your prediction and reason |
| --- | --- |
| Intended lesson CA + expected name `localhost` | I predict a verification of "ok". The CA is what we intend and the expect name of 'localhost' will pass verification. |
| Same certificate and CA + expected name `wrong.test` | I predict a failed verification. The certificate is the same, but the expected name is changed so verification will fail. |
| Same certificate and expected name + unrelated CA | I predict a failed verification. The certificate and expected name are the same, but the CA isn't who we expect so it will fail verification. |
| Same intended inputs, but no listener on port 8443 | I predict the connection will fail. If the port isn't listening, no connection can be made in order to verify the certificate. |

## Guided Steps

### Part A - Prepare the Key, Request, and Issuer

#### Step 1 - Check the Environment

This first box checks your account, the installed tools, the clock, and whether our numbered door is already in use. It also makes the Week 9 folder if it does not exist. **It does not start the practice service.**

```bash
whoami
openssl version
command -v ss
date -u
mkdir -p ~/cloud-heights/week9-digital-trust
test -w ~/cloud-heights/week9-digital-trust && printf 'Workspace writable\n'
ss -ltn 'sport = :8443'
```
**What you should see, in order**:

1. `analyst` — your account name.
2. An OpenSSL version beginning with `3.` — the tool is installed.
3. A file path ending in `ss` — the tool for listing waiting services is available.
4. Today’s date and time in **UTC**, a shared time standard. It can differ from your local clock by your time-zone offset.
5. `Workspace writable` — you can save files in the Week 9 folder.

```text
Account name shown: analyst
OpenSSL version shown: OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
Path to the ss tool: /usr/bin/ss
UTC date and time shown: Thu Sep 24 02:12:01 UTC 2026
Workspace writable result: Workspace writable
Port 8443 listener result (blank output means nothing is listening): State          Recv-Q          Send-Q                   Local Address:Port                   Peer Address:Port         Process
```

6. A heading with no service row below it — port 8443 is free.

**Stop and ask for help** if an item is missing, the date is wrong, or a service row appears. Do not close an unknown program or change the clock yourself.

```text
Account: analyst
OpenSSL version: OpenSSL 3.0.2
UTC clock: Thu Sep 24 02:12:01 UTC 2026
Port 8443 free? Evidence: Yes, port 8443 is free. After the [ss -ltn 'sport = :8443'] command, I received a heading with no service row below it.
```

#### Step 2 - Create a Fresh Practice Folder

This box makes a new folder for this attempt. **Directory** means folder. `mktemp` chooses a fresh name so you do not overwrite earlier work. `umask` restricts who can read newly created files. The four inner folders will hold the badge office files, applications, finished badges, and check results.

```bash
umask 077
W9_RUN=$(mktemp -d "$HOME/cloud-heights/week9-digital-trust/practice-XXXXXX")
cd "$W9_RUN"
mkdir ca requests certificates verification
printf '%s\n' "$PWD"
```

```text
My exact practice path: ~/cloud-heights/week9-digital-trust/practice-rhX0Fi$
```

**What you should see:** a path ending in `practice-` plus a random ending. Copy that exact path into your answer above. It will be different for each attempt.

**To resume later:** type `cd`, then a space, then paste your recorded path and press Enter. Type `pwd` and press Enter to confirm you are in that folder. Do not run Step 2 again just to resume; that would create a different attempt.

#### Step 3 - Create the Service Key and CSR

First make a new key pair for Ivy’s practice service. **RSA** is the kind of key we are making; **2048** is its size in bits. These values are provided for you. This box saves the private key and writes a separate file containing only the public key. Do not reuse a key you use to log in.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out requests/service.key.pem
chmod 600 requests/service.key.pem
openssl pkey -in requests/service.key.pem -pubout -out requests/service.public.pem
```
**What you should see:** progress symbols may appear, then the prompt returns. Some lines finish without printing anything. The private-key file is restricted to your account by `chmod 600`. It has no password protection for this temporary practice exercise; real systems need a planned way to protect their keys.

**Do not open or print `service.key.pem`.** `cat` displays a file’s contents, so use it only on the exact public-information files listed in these instructions.

Next make the badge application: the **CSR**. It includes the service’s public key, its requested name, and a signature made with its private key. `localhost` is the practice name for this computer. **SAN** means the list of names the certificate should cover. This request asks for `localhost`. The box also checks the request’s signature and displays the request, which contains public information.

```bash
openssl req -new -sha256 -key requests/service.key.pem -out requests/service.csr.pem -subj "/O=CyberFoundations Lab/CN=localhost" -addext "subjectAltName=DNS:localhost"
openssl req -in requests/service.csr.pem -noout -verify
openssl req -in requests/service.csr.pem -noout -text > verification/csr-inspection.txt
cat verification/csr-inspection.txt
```
Expected: a request-signature verification message such as `Certificate request self-signature verify OK`, plus requested `DNS:localhost` in the inspection. A CSR contains public information and a signature, not the private key. Record the actual output, not this example.

```text
Requested subject and SAN: The requested subject is 'localhost' and the SAN is 'DNS: localhost'
CSR signature-check result: Certificate request self-signature verify OK
What the successful signature check tells me about the request: The successful signature check tells me the the public key on the request has a cryptographic relationship with the service's private key.
Why signing an application does not prove permission to use someone else’s website name: Signing an application does not prove permission to use someone else's website name. It only proves that the requester controls the private key matching the public key inside the request, not that they have legal permission or ownership of the domain name listed inside it.
```

#### Step 4 - Model the Issuing Office

Now pretend you work at the badge office. The office needs its **own** key pair, separate from the service’s key. This box makes that pair and the office’s certificate. The office signs its own certificate; that is called **self-signed**. Printing and signing your own badge does not make other people accept it. Later, we will tell our checking tool to accept this particular office for a particular test.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/lesson-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/lesson-ca.key.pem -out ca/lesson-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Week 9 Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/lesson-ca.key.pem
openssl x509 -in ca/lesson-ca.cert.pem -noout -subject -issuer -dates
```
**What you should see:** Subject and Issuer both name `Week 9 Lesson CA`, followed by dates. That is expected for this self-signed office certificate. Here the office will sign the service badge directly; there is no middle office (intermediate CA). Your browser and computer’s accepted-office lists have not changed.

```text
Lesson CA subject shown: subject=O = CyberFoundations Lab, CN = Week 9 Lesson CA
Lesson CA issuer shown: issuer=O = CyberFoundations Lab, CN = Week 9 Lesson CA
Lesson CA validity dates: notBefore=Sep 24 02:44:28 2026 GMT
notAfter=Sep 24 02:44:28 2027 GMT
Why a self-signed office certificate is not automatically accepted: A self-signed office certificate is not automatically accepted because self-signing doesn't automatically create public trust. A verifier has to accept the certificate.
```

**Pause point:** save your worksheet and practice path before Part B. Your keys and request are saved on the VM.

### Part B - Issue, Inspect, and Verify

#### Step 5 - Set the Badge Rules and Issue the Certificate

This box writes the office’s rules into a small settings file. The rules say: “This badge is for a service called `localhost`, for use as a web server. It cannot issue other badges.”

Copy the entire box, including the last `EOF` on its own line. `EOF` tells the terminal “the file text ends here.” If you see a `>` prompt while entering the block, the terminal is waiting for the remaining lines. Do not paste the next command until the complete block has finished.

You do not need to memorize these settings. `CA:FALSE` means “not an issuing office”; `serverAuth` means “may identify a server”; `DNS:localhost` is the allowed name. The office chooses the final rules instead of accepting everything an applicant might ask for.

```bash
cat > ca/server-ext.cnf <<'EOF'
[server_cert]
basicConstraints=critical,CA:FALSE
keyUsage=critical,digitalSignature,keyEncipherment
extendedKeyUsage=serverAuth
subjectAltName=DNS:localhost
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid,issuer
EOF
```
**What you should see:** the usual prompt returns; this settings-file step normally prints nothing.

Now issue the badge. The next box uses the **office’s private key** to sign the certificate, gives it a 30-day date period, saves it, and displays it. Its serial number is a randomly chosen tracking number; yours need not match another student’s.

```bash
openssl x509 -req -in requests/service.csr.pem -CA ca/lesson-ca.cert.pem -CAkey ca/lesson-ca.key.pem -set_serial "0x$(openssl rand -hex 16)" -days 30 -sha256 -extfile ca/server-ext.cnf -extensions server_cert -out certificates/service.cert.pem
openssl x509 -in certificates/service.cert.pem -noout -text > verification/certificate-inspection.txt
cat verification/certificate-inspection.txt
```
**What you should see:** Subject includes `localhost`; Issuer includes `Week 9 Lesson CA`; SAN includes `DNS:localhost`; Basic Constraints says `CA:FALSE`; and Extended Key Usage lists TLS Web Server Authentication. If a required value differs, stop and ask for help before the next step.

```text
SAN shown in the certificate inspection: X509v3 Subject Alternative Name: 
                DNS:localhost
Basic Constraints shown in the certificate inspection: X509v3 Basic Constraints: critical
                CA:FALSE
Extended Key Usage shown in the certificate inspection: X509v3 Extended Key Usage: 
                TLS Web Server Authentication
Any difference from the expected result (write "none" if there is none): None
```

Use these reminders to fill the table: **Subject** describes the badge holder; **Issuer** names the signing office; **SAN** lists allowed names; **Not Before/Not After** give the start/end dates; **algorithm** means calculation method; **Basic Constraints** says whether this is an issuing office; **Key Usage/EKU** describe allowed jobs. The website’s key and the office’s signature have different jobs.

Your dates, tracking number, and key details will differ from classmates’ files. Copy your finished certificate’s values, not just the application’s values.

| Field | Issued value | Why it matters |
| --- | --- | --- |
| Subject | Subject: O = CyberFoundations Lab, CN = localhost | It identifies who the certificate is for. The subject should match the CN in the CSR |
| Issuer | Issuer: O = CyberFoundations Lab, CN = Week 9 Lesson CA | Identifies who signed the certificate. Needs be be a trusted source. |
| SAN | X509v3 Subject Alternative Name: DNS:localhost | Listed other identities associated with the certificate. |
| Not Before / Not After | Not Before: Sep 24 13:48:16 2026 GMT and Not After : Oct 24 13:48:16 2026 GMT | The time frame that the certificate is valid. |
| Subject key algorithm and size | Public Key Algorithm: rsaEncryption, Public-Key: (2048 bit) | The algorithm used for the subject's public key. |
| Certificate signature algorithm | Signature Algorithm: sha256WithRSAEncryption | The algorithm used for the issuers private key. |
| Basic Constraints | X509v3 Basic Constraints: critical, CA:FALSE | States whether the certificate is a CA or not, can it issue certificates or not. |
| Key Usage / Extended Key Usage | X509v3 Key Usage: critical, Digital Signature, Key Encipherment   X509v3 Extended Key Usage: TLS Web Server Authentication | States what the certificate is allowed to do. |

#### Step 6 - Check That the Same Public Key Was Carried Through

Did the public key from Ivy’s key pair make it into the application and then the finished badge? This box copies out the public information and calculates a **SHA-256 fingerprint** for each public-key file. A fingerprint is a calculated label we can compare. This box does not display private keys.

```bash
openssl req -in requests/service.csr.pem -pubkey -noout > requests/csr.public.pem
openssl x509 -in certificates/service.cert.pem -pubkey -noout > certificates/service.public.pem
sha256sum requests/service.public.pem requests/csr.public.pem certificates/service.public.pem
```
**What you should see:** three lines beginning with the same long string of letters and numbers. Those strings are the fingerprints, also called **digests**. Matching strings show that the same public key appears in the original public-key file, the CSR, and the certificate. They do not tell us whether the name, dates, or issuing office should be accepted. If the strings differ, stop and ask for help before continuing.

```text
Fingerprint for requests/service.public.pem: 502221e80c6a58709a3100b46e49cd5696eb06ede7fc61ca7b6b33d9ae7cc873  requests/service.public.pem
Fingerprint for requests/csr.public.pem: 502221e80c6a58709a3100b46e49cd5696eb06ede7fc61ca7b6b33d9ae7cc873  requests/csr.public.pem
Fingerprint for certificates/service.public.pem: 502221e80c6a58709a3100b46e49cd5696eb06ede7fc61ca7b6b33d9ae7cc873  certificates/service.public.pem
Do all three match, and what does that show: Yes, all three match. It shows that the same public key appears in the certificate, the request for the certificate (CSR), and the public key file.
```

#### Step 7 - Run the Passing Verification

Now check the saved certificate without connecting to a running service. This is an **offline check**: it reads files on your VM.

Read the main choices in the command before running it:

| Command part | What we are asking |
|---|---|
| `-CAfile ca/lesson-ca.cert.pem` | Accept this badge office for this check. This chosen starting point is called a trust anchor. |
| `-no-CApath -no-CAstore` | Do not also use the computer’s default certificate folders or store. |
| `-purpose sslserver` | Check that the badge is suitable for a server. |
| `-verify_hostname localhost` | Check that it covers the name `localhost`. |

OpenSSL also uses the computer’s clock to check dates. The last three lines save the result number, show the message, and print that number. An **exit status** of `0` means the command succeeded; another number means it did not. Read the message to learn why. `> ... 2>&1` saves the command’s messages in a text file; it is included for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/pass.txt 2>&1
W9_STATUS=$?
cat verification/pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** `certificates/service.cert.pem: OK` and exit status `0`. This is our working starting test, also called the **baseline**. If it fails, stop and ask for help. The later wrong-input tests are meaningful only after this starting test works.

```text
Command message shown by verification/pass.txt: certificates/service.cert.pem: OK
Exit status printed: Exit status: 0
What this baseline result proves: It means the command was successful. The ca/lesson-ca.cert.pem is now a trust anchor.
```

#### Step 8 - Ask for the Wrong Name on Purpose

Keep the same badge and office, but ask whether the badge covers `wrong.test`. It was issued for `localhost`, so these names should not match. Copy this complete box; the change is already made for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname wrong.test certificates/service.cert.pem > verification/wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
Expected: hostname mismatch and a nonzero exit status. The leaf and CA did not change. `wrong.test` is just an expected-name input; this offline command makes no connection to that name.

```text
Command message shown by verification/wrong-name.txt: O = CyberFoundations Lab, CN = localhost
error 62 at 0 depth lookup: hostname mismatch
error certificates/service.cert.pem: verification failed
Exit status printed: Exit status: 2
Why the check refused this name: The check refused this name because the badge/certificate was issued to 'localhost' not 'wrong.test', so the verification failed as expected.
```

#### Step 9 - Ask a Different Office to Vouch for the Same Badge

First make a second pretend office with its own key. It did not issue our service’s badge. This is a **negative test**: we intentionally give wrong information to see whether the check refuses it. The first box makes the second office; the next box checks the unchanged service certificate using only that second office.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/unrelated-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/unrelated-ca.key.pem -out ca/unrelated-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Unrelated Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/unrelated-ca.key.pem
```

```bash
openssl verify -CAfile ca/unrelated-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/wrong-ca.txt 2>&1
W9_STATUS=$?
cat verification/wrong-ca.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a message such as `unable to get local issuer certificate`, and an exit status other than `0`. In plain language: “The office you gave me cannot vouch for this badge.” That refusal is expected. A missing-file error is a different problem; ask for help if you see one. Do not install these offices into your browser or computer’s accepted list.

```text
Command message shown by verification/wrong-ca.txt: O = CyberFoundations Lab, CN = localhost
error 20 at 0 depth lookup: unable to get local issuer certificate
error certificates/service.cert.pem: verification failed
Exit status printed: Exit status: 2
Why the unrelated office could not vouch for this badge: The unrelated office could not vouch for this badge because it did not issue the badge/certificate, another office (Week 9 Lesson CA) issued the badge.
```

**Pause point:** record all three results below before Part C. Keep the same service certificate throughout these tests.

| Test | Leaf file | CA file | Expected name | Actual result / exit status | Explanation |
| --- | --- | --- | --- | --- | --- |
| Passing baseline | service.cert.pem | lesson-ca.cert.pem | localhost | Exit status: 0 | The verification passed. The CA and expected name matched. |
| Wrong name | service.cert.pem | lesson-ca.cert.pem | wrong.test | Exit status: 2 | The verification failed. The expected name didn't match with the CN on the certificate. |
| Wrong CA | service.cert.pem | unrelated-ca.key.pem | localhost | Exit status: 2 | The verification failed. The CA couldn't vouch for the badge because it didn't issue the badge/certificate, the CA used for verification is incorrect. |

### Part C - Try a Protected Connection on This VM

So far we checked saved files. Now we will connect two programs on the same VM.

- The **server** waits for a visitor. The **client** makes the visit and checks the badge.
- `127.0.0.1` is the address meaning “this same computer.” Connecting back to yourself is called **loopback**.
- `8443` is the numbered door, or **port**, where our server waits.
- `localhost` is the name we expect on its certificate. An address tells the client where to go; the expected name tells it which identity to check.
- **TLS 1.3** is the protected-connection version we will use. Its handshake is the initial exchange where the programs check identity and arrange message-protection keys.

Keep two terminal windows open: **A runs the server; B runs the checks.** Both must connect to your assigned VM.

#### Step 10 - Start the Listener in Terminal A

Use your existing terminal as **Terminal A**. Make sure you are in the practice folder from Step 2. This box starts your waiting service:

```bash
openssl s_server -accept 127.0.0.1:8443 -cert certificates/service.cert.pem -key requests/service.key.pem -www -tls1_3
```
**What you should see:** `ACCEPT`, and the usual prompt does not return. This is normal: the server is waiting. No client has checked its identity yet. Leave this window alone and continue in Terminal B. Keep the address exactly `127.0.0.1` so the service is available only on this VM. Do not change it to `0.0.0.0` or change network rules.

```text
Message shown in Terminal A after starting the listener: Using default temp DH parameters
ACCEPT
Time you started the listener: 11:01 AM EDT
```

#### Step 11 - Check the Connection in Terminal B

Open a second terminal to the **same VM**. Use `cd` with the exact practice path you recorded in Step 2. Confirm it with `pwd`; the `ca/` and `certificates/` directories must belong to that run. Then execute:

```bash
ss -ltn 'sport = :8443'
```
Expected: a listener at `127.0.0.1:8443`. Save that evidence before the connection test.

```text
Listener row shown by ss: State          Recv-Q          Send-Q                   Local Address:Port                   Peer Address:Port         Process         
LISTEN         0               4096                         127.0.0.1:8443                        0.0.0.0:*
```

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-pass.txt 2>&1
W9_STATUS=$?
cat verification/tls-pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** TLS 1.3, `Verification: OK`, and exit status `0`. If not, confirm Terminal A is still waiting and both terminals use the same practice folder. Ask for help if it still differs.

Two command options have different jobs. `-servername localhost` tells the server which service name we want; this label is called **SNI**. `-verify_hostname localhost` checks whether the certificate actually covers the name. Requesting a name is not the same as checking it. `-verify_return_error` tells the client to stop if the certificate check fails. The short output also shows a **cipher**: the chosen method for protecting messages.

Now change only the expected name, keeping SNI and all other inputs the same:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname wrong.test -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/tls-wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a certificate/name mismatch message and a status other than `0`. Reaching the server’s door is not enough; the badge must also pass the name check. If the message says “connection refused,” the server is not waiting, so that is not evidence of the intended name rejection.

```text
Listener address and port: 127.0.0.1:8443
Connected destination: The same VM that I'm on, the loopback.
Expected identity for the passing test: localhost
SNI value: localhost
CA file used: lesson-ca.cert.pem
Negotiated TLS version and cipher from the passing output: Protocol version: TLSv1.3
Ciphersuite: TLS_AES_256_GCM_SHA384
Passing verification message and exit status: Verification: OK
Exit status: 0
Failing verification message and exit status: verify error:num=62:hostname mismatch
40D75FABF0730000:error:0A000086:SSL routines:tls_post_process_server_certificate:certificate verify failed:../ssl/statem/statem_clnt.c:1883:
Exit status: 1
What changed between the two tests: The hostname changed
Explain each job: the certificate links a name to a public key; the handshake checks use of the matching private key; newly arranged traffic keys protect messages. How are these different? The public key in a certificate is for verifying the subject's (requestor of the certificate) private key. The handshake check is when the client verifies the signature of the server using the server's public key, which will prove the server has the matching private key. Traffic keys protect messages by encrypting the data while it's being exchanged between the server and client.
```

#### Step 12 - Stop Only Your Listener

In Terminal A, press **Ctrl+C**. In Terminal B, rerun `ss -ltn 'sport = :8443'`; no listener row should remain. Do not use a broad process-kill command.

In Terminal B, use the original passing inputs but save this stopped-listener result in its own file:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/no-listener.txt 2>&1
W9_STATUS=$?
cat verification/no-listener.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```

**What you should see:** “connection refused” and a status other than `0` when no service is waiting. There is no one at the door, so the certificate check never gets that far. Keep this result separate from the earlier wrong-badge result.

```text
ss output after you pressed Ctrl+C: >
Message shown by verification/no-listener.txt: 4077614A4A790000:error:8000006F:system library:BIO_connect:Connection refused:../crypto/bio/bio_sock2.c:125:calling connect()
4077614A4A790000:error:10000067:BIO routines:BIO_connect:connect error:../crypto/bio/bio_sock2.c:127:
connect:errno=111
Exit status printed: Exit status: 1
Why this failure is different from a badge failing its check: This failure is different because it's a connection issue (the server isn't listening), not a badge failing a verification check due to incorrect details.
```

**Pause point:** the server is now stopped. Save your worksheet and screenshots before Part D.

### Part D - Explain Problems and Collect Your Work

#### Step 13 - Interpret Four Conditions

For each row, explain what went wrong, what message or field you would look at, what you would correct, and which check you would repeat. **Retest** means try the check again after a correction. Use your earlier wrong-name, wrong-office, and stopped-server results to reason about rows 1, 3, and 4. Row 2 is a pretend situation to explain in writing; do not change the VM clock or certificate dates.

| Condition | Which check cannot pass? | What would you look at? | What would you fix, and why? | Which check would you repeat? |
| --- | --- | --- | --- | --- |
| `localhost` is absent from a service's SAN | The name of the subject will not pass | I would look at the SANs listed on the certificate and see if 'localhost' is listed | I would fix the SAN list to include 'localhost' because if the certificate is made for 'localhost' then that name should be included on the list of names/websites in the SAN field of the certificate | I would repeat the check of looking at the subject/website name that I intended to reach and compare it to the SANs listed on the certificate |
| Not After is in the past relative to the correct clock | The validity dates of the certificate will not pass | I would look at the date, time, and timezone on my computer and the 'Not After' validity date on the certificate | I would issue a new certificate with current Not Before and Not After dates so the certificate will be valid and not expired | I would repeat comparing the date, time, and timezone of my computer to the Not After date listed on the certificate |
| An unrelated CA file is supplied | The root/intermediate CA check will not pass | I would look at the certificate issuer chain. I'd look at the trust store to see if the CA is listed there. | I would verify that the CA is a trustworthy source and get confirmation on adding the CA as a trust anchor. This will help if we see any other certificates from the same CA in the future. | I'd check the trust store to make sure the CA is on that list and I'd check the certificate chain again. |
| No listener exists at `127.0.0.1:8443` | A connection check will not pass | I would check the server to see if  it's on and running. I would check to see if port 8443 is free to listen for a connection. I would check the firewall rules to see if there's an inbound rule that's blocking traffic on port 8443. | I would turn on the server and make sure it's running and available to make sure a connection is possible. I would run a command for the server to listen on port 8443 to test the connection. I would create an inbound rule to allow traffic on port 8443 so a connection to the port is possible. | I'd check the server again to make sure it's running, the port to make sure it's open, the server to make sure it's listening, and the firewall rules to ensure traffic to port 8443 is allowed. |

A badge may need replacing when its dates run out. Getting a new certificate is called **renewal**. The service must also be set up to use the new file; that is **deployment**. An office can cancel a certificate before its end date; that is **revocation**. Our file checks did not contact an online cancellation service.

In your own words, explain why getting a replacement file is not enough until the server uses it. Then explain why our passing file check does not prove that someone checked an up-to-date cancellation list.

```text
Getting a replacement file/certificate isn't enough until the server uses it because it has to be deployed first. Until the replacement gets deployed the old file is still in use. Our passing file check does not prove that someone checked an up-to-date cancellation list because a file check isn't the same as checking a revocation list or contacting an online cancellation service.
```

#### Step 14 - Compare the Real Website with Your Practice Service

Use your dated Lab 01 observation and your actual local certificate.

| Comparison | Public website from Lab 01 | Isolated lesson service |
| --- | --- | --- |
| Intended hostname / SAN | example.com | localhost |
| Issuer and chain roles | Cloudflare TLS Issuing ECC CA 3 O = SSL Corporation C = US is the Leaf role | CyberFoundations Lab, CN = Week 9 Lesson CA is the only CA, it's the Root. |
| Validity interval | Not Before 7/29/26, 6:10:08 PM EDT and Not After 10/27/26, 6:17:21 PM EDT | Not Before: Sep 24 13:48:16 2026 GMT and Not After : Oct 24 13:48:16 2026 GMT |
| Allowed purpose | TLS WWW Server Authentication | X509v3 Key Usage: critical, Digital Signature, Key Encipherment   X509v3 Extended Key Usage: TLS Web Server Authentication |
| Who accepts the top issuing office, and how is that choice made? | The verifier accepts the top office and that choice is made by looking at the certificate hierarchy and certificate fields to confirm the data matches the intended service and is valid during our current time frame. Cloudflare is publicly trusted. | I am the issuing office so I accepted myself. The choice was made by me because I'm the issuer and the verifier |
| Where does the service run, and how did you connect? | The service runs at example.com and I connected by going to the website. | The service runs on my VM and I connected by requesting the server to listen via loopback 127.0.0.1:8443 and opened a 2nd VM to test if the service was listening |
| Evidence actually observed | The certificate issued to example.com via the my Chrome browser. The certificate hierarchy and certificate fields of the certificate. | The certificate shown in my VM terminal. |
| What does this evidence NOT tell you? | It doesn't tell me if the certificate has been revoked. | It doesn't tell me that other people's browsers will accept the certificate. |

## Stop & Check

- Can you explain the separate jobs of the service’s secret key, its application (CSR), the office’s secret key, and the finished badge (certificate)?
- Does the issued SAN contain the intended name?
- Do all three public-key fingerprints match, with no private key displayed?
- Did the correct-input check pass before you tried the wrong inputs?
- Did each failure occur for the predicted reason rather than because of a missing file?
- Did the service wait only at `127.0.0.1:8443`, and did you stop it afterward?

## Test

Your evidence must show: three matching public-key fingerprints; a passing file check; refusal of the wrong name; refusal of the wrong office; a passing live TLS connection; refusal of the wrong name during TLS; and a refused connection after stopping the service. For each check, include the command you ran, the message, and the exit-status number. A number other than `0` is not enough by itself: explain the message. Do not copy the terminal’s prompt into a command.

## Capture Evidence

Take screenshots of the public CSR/certificate inspection and each required test outcome. Capture command inputs and result together when possible. Do not screenshot private keys or unfiltered verbose TLS session dumps. Copy exact relevant text into the worksheet as well, so another learner can follow your reasoning.

## Explain

Write 5–7 sentences telling the story of what you did. Start with “First I made…”, then describe the badge application, the issuing office’s signature, your inspection, your file checks, and the live connection. Say which office you told the client to accept. Include one wrong-input test, why it failed, and what you kept the same.

```text
I first made a folder to save all my work into. Then I created a key pair for my service. I created a CSR that contained my service's public key and signed with my service's private key. I created a pretend certificate authority and a key pair for that CA. I used the pretend CA to issue a certificate to localhost. I inspected the SAN, basic constraints, validity dates, and extended key usage of the certificate. I checked to see that the same public key appeared on the CSR, the certificate, and the public key file itself. I told the client to accept the Lesson CA office. The wrong-input test failed because the intended name (wrong.test) didn't match the name/service listed on the certificate (localhost), all other fields stayed the same. I checked for a connection by using a loopback method setting up one VM as the server and a 2nd VM as the client.
```

## Required Evidence

Save screenshots under `assets/screenshots/week-09/`:

- `week09-lab02-csr-check.png`
- `week09-lab02-issued-certificate.png`
- `week09-lab02-key-correspondence.png`
- `week09-lab02-verify-pass.png`
- `week09-lab02-verify-wrong-name.png`
- `week09-lab02-verify-wrong-ca.png`
- `week09-lab02-loopback-listener.png`
- `week09-lab02-tls-pass.png`
- `week09-lab02-tls-wrong-name.png`
- `week09-lab02-no-listener.png`

Add numbered image suffixes if necessary for readability. Keep `verification/*.txt` on the VM for your own reference; publishing those logs is optional only after a content review. Do not upload any `.key.pem` file, even though it is a classroom key.

### Evidence Uploads (required)

![week09-lab02-csr-check.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-csr-check.png)

**Caption - CSR inspection:** The certificate signing request application is shown.

![week09-lab02-issued-certificate.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-issued-certificate.png)

**Caption - issued certificate:** Certificate/badge issued by the pretend CA to localhost

![week09-lab02-key-correspondence.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-key-correspondence.png)

**Caption - public-key fingerprints:** Shows comparison of the SHA 256 fingerprints for the public key file, the public key on the CSR, and the public key on the certificate

![week09-lab02-verify-pass.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-verify-pass.png)

**Caption - passing offline check:** Checking the saved certificate

![week09-lab02-verify-wrong-name.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-tls-wrong-name.png)

**Caption - wrong-name refusal:** Showing that using the wrong name to verify the certificate doesn't work

![week09-lab02-verify-wrong-ca.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-verify-wrong-ca.png)

**Caption - wrong-office refusal:** Showing that using the incorrect office to verify a certificate issued by a different office will not pass verification

![week09-lab02-loopback-listener.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-loopback-listener.png)

**Caption - loopback listener:** Showing that the server is set to listen

![week09-lab02-tls-pass.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-tls-pass.png)

**Caption - passing TLS connection:** Showing a TLS connection was established

![week09-lab02-tls-wrong-name.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-tls-wrong-name.png)

**Caption - TLS wrong-name refusal:** Showing certificate verification failure because the wrong name was used

![week09-lab02-no-listener.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-no-listener.png)

**Caption - stopped listener:** Showing that the server is no longer listening

### Optional Extra Evidence (leave blank if you do not need it)

**Caption - extra image 1:**

**Caption - extra image 2:**

## Analysis Questions

**Analysis Question 1.** Who signs the badge application: the service or the office? Who signs the finished badge? Why does signing an application not prove you have permission to use someone else’s website name? Write at least 3 sentences.

```text
The service (requestor) signs the badge application. The office signs the finished badge. Signing the application does not prove you have permission to use someone else's website name because anyone can submit an application to request a certificate. Signing the application does not mean you have legal permission or ownership of the domain name listed inside it.
```

**Analysis Question 2.** You kept the same certificate but changed the name or office used for the check. Why did the answer change? Use the messages from both wrong-input tests. Write at least 3 sentences.

```text
Using the same certificate, but changing the name used to verify the certificate resulted in an error message (error 62 at 0 depth lookup: hostname mismatch) because the names don't match. Using the same certificate, but changing the office used for the check resulted in a failure as well. The error message 'error 20 at 0 depth lookup: unable to get local issuer certificate' appears because the CA used for verification is not the same CA that issued the certificate.
```

**Analysis Question 3.** Explain the difference between a server waiting at a door, a badge passing its checks, and messages traveling through a protected connection. If you see “connection refused,” what would you check first, and why? Write at least 3 sentences.

```text
A server waiting at a door means it's ready to receive a connection from a client. A badge passing its checks means that a certificate is verified after checking specific fields in the certificate. Messages traveling through a protected connection means that the data is encrypted. If I see 'connection refused' I would check to see if the port I'd check to see if the port is listening in order to receive a connection.
```

## Submission Checklist

- [x] Fresh practice path recorded; no previous files or SSH configuration changed

- [x] Dedicated service key and CSR created; issuer role explained

- [x] Issued certificate fields and matching public components recorded

- [x] Offline success, wrong-name failure, and wrong-CA failure explained

- [x] Loopback listener address and passing/failing TLS evidence captured

- [x] Listener stopped; connection failure distinguished from certificate failure

- [x] Four-condition diagnosis and public/local comparison completed

- [x] Analysis questions and walkthrough written in my own words

- [x] Lab 01, notes, and reflection included for Portfolio Deliverable 3

- [x] No private keys, credentials, or access URLs in the submission

- [x] Worksheet committed to `week-09/labs/lab-02-build-and-test-digital-trust.md`

## GitHub / Lab Portal Submission

Your **portfolio repository** is your course-work folder on GitHub. A **commit** saves a set of changes there. Use the editing or upload method you practiced in Week 7; ask for a demonstration if you need one. Upload only the listed documents and reviewed images, never the whole VM folder.

1. Fill in this worksheet on this page and press **Save Progress** as you work. Your answers are stored in the portal and reload when you return.
2. Press **Submit to GitHub**. The portal commits the finished worksheet to the submission path shown at the top of your connected portfolio repository.
3. Add only the reviewed evidence images under `assets/screenshots/week-09/`, or paste their image addresses in the evidence fields above.
4. Complete the Week 9 Notes and Reflection worksheets in the portal the same way. Use the submissions guide to assemble Portfolio Deliverable 3.
5. Open the committed Markdown and every image on GitHub to confirm formatting and privacy.

*CyberVisionaries Institute · CyberFoundations · Tier I*
