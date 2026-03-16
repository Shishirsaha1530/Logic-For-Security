Below is a **clean GitHub README** based on your report. It is written in **typical GitHub project style** so it looks professional if you upload your protocol project repository.

You can copy and paste this directly into **`README.md`**.

---

# Secure Photo Authorization Protocol

**Course:** 02244 Logic for Security
**Institution:** Technical University of Denmark (DTU)
**Group 100**

## Authors

* Ahsunalla Wahidi — s184858
* Shishir Saha — s250162
* Md Sabbir Hossain — s250143

---

# Project Overview

This project presents the **design and formal analysis of a secure authorization protocol** that allows a user to grant a third-party service controlled access to their photos stored on a remote server.

The goal is to allow a **photo book service** to access selected photos without requiring the user to download and upload the files manually.

Instead, the protocol allows the service to retrieve the photos **directly from the storage server** using a secure authorization token issued by a trusted identity provider.

The protocol was designed and verified using formal security analysis techniques.

---

# System Scenario

The system consists of four participants:

| Entity                       | Role                                                |
| ---------------------------- | --------------------------------------------------- |
| **A (User)**                 | Owner of the photos                                 |
| **B (Photo Storage Server)** | Stores the user's photos                            |
| **P (Photo Book Service)**   | Service that prints the photo book                  |
| **IdP (Identity Provider)**  | Authenticates users and issues authorization tokens |

### Goal

Allow **P** to access specific photos stored on **B**, with permission granted by **A** and verified by **IdP**, while ensuring security and privacy.

---

# Functional Requirements

The protocol must:

* Allow **User A** to authorize **Service P** to access photos stored on **Server B**
* Allow **direct photo retrieval** from the storage server
* Restrict access to **read-only permissions**
* Require authentication through the **Identity Provider**
* Ensure that **Server B verifies authorization tokens**

---

# Security Requirements

The protocol must guarantee:

* Confidentiality of user photos
* Confidentiality of the user password
* Authorization limited to selected resources
* Authenticity of tokens issued by the Identity Provider
* Protection against impersonation and forged permissions

---

# Protocol Development

The protocol was developed iteratively during **Weeks 2–6** of the course.

## Week 2 – Initial Protocol Design

Initial authorization protocol using:

* public-key encryption
* digital signatures
* authorization tokens

Formal verification using **OFMC** revealed an **attack** where an intruder could obtain a valid authorization token without authenticating as the user.

### Issue Identified

The **Identity Provider did not authenticate the user before issuing tokens.**

---

## Week 3 – Authentication Integration

A **challenge–response authentication mechanism** was introduced.

Process:

1. User sends authorization request
2. Identity provider sends nonce
3. User responds with hash of password and nonce
4. Identity provider verifies the response before issuing the token

This ensures that only the legitimate user can obtain authorization tokens.

---

## Week 4 – Type-Flaw Protection

Explicit **message tags** were added to prevent **type-flaw attacks**.

Examples of tags:

* `fauthreq`
* `fchall`
* `fauthproof`
* `ftok`
* `fdeliver`
* `fuse`
* `fresp`

These tags ensure that each message has a unique format and cannot be misinterpreted.

A **public-key retrieval mechanism** was also added through the identity provider.

---

## Week 5 – Secure Channel Abstraction

To simplify the protocol specification:

* explicit cryptographic operations were replaced with **secure communication channels**
* channels provide built-in authentication and confidentiality

Example channel notation:

```
[A] *->* idp : message
```

This represents an authenticated and confidential communication channel.

---

## Week 6 – Password Security Analysis

The protocol was evaluated assuming the password is a **guessable secret**.

Two variants were tested:

### Insecure Variant

```
A -> idp : req
idp -> A : nonce
A -> idp : hash(password, nonce)
```

This allowed an attacker to perform an **offline dictionary attack**.

### Secure Variant

Authentication was moved inside a **secure channel**:

```
[A] *->* idp : login, A, P, B, pw(A,idp)
```

Since the password is never observable, dictionary attacks become impossible.

---

# Final Protocol Flow

```
1. A -> IdP  : login request
2. IdP -> A  : signed authorization token
3. A -> P    : token forwarding
4. P -> B    : photo request with token
5. B -> P    : encrypted photos
```

The storage server verifies the **signature of the identity provider** before releasing the photos.

---

# Verification

The protocol was verified using:

* **AnB specification language**
* **OFMC model checker**

### Security Properties Verified

* Authentication of authorization tokens
* Secrecy of user photos
* Secrecy of the user password
* Resistance to impersonation attacks

The analysis confirmed that the final protocol satisfies all required security goals under the **Dolev–Yao attacker model**.

---

# Assumptions

The analysis assumes:

* Perfect cryptography (Dolev–Yao model)
* Honest identity provider
* Participants follow the protocol correctly
* Private keys remain secret

---

# Limitations

Formal verification does not consider:

* implementation vulnerabilities
* side-channel attacks
* real-world cryptographic weaknesses
* unbounded protocol sessions

---

# Tools Used

* **OFMC (Open-source Fixedpoint Model Checker)**
* **AnB protocol specification language**

---

# AI Assistance

AI tools (including **Gemini**) were used to assist with:

* improving documentation
* organizing explanations
* refining parts of the protocol specification

All technical design and verification results were developed and validated by the project group.

---

# Conclusion

This project demonstrates how **formal verification techniques** can be used to design and analyze secure protocols.

Through iterative improvements and formal analysis, the final protocol ensures secure delegation of photo access while protecting user credentials and data confidentiality.

---

If you want, I can also give you a **much more professional GitHub README (with diagrams, badges, and protocol flow visuals)** that looks like a **real security research repository**. It will make your project look **much stronger on GitHub and CV**.
