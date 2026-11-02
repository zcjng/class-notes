# 🗄️ HITCON CTF — "simple-drive": Predictable PRNG → Signature Forgery → Symlink Escape

> [!ABSTRACT]
> A cloud-storage web app signs its backups with a "master key" using a random nonce `k`. That `k` comes from **MT19937**, and the server leaks PRNG output through its **session IDs**. Predict the nonce → recover the master key → forge a signed backup → smuggle a **symlink to `/flag`** through `/restore`.

---

## 🎯 The Attack Chain at a Glance

| Phase      | Abused Feature                      | What We Get                         |
| :--------- | :---------------------------------- | :---------------------------------- |
| 1. Recon   | The website + its routes            | Map of the attack surface           |
| 2. Leak    | `/register`, `/login` (session IDs) | MT19937 internal state              |
| 3. Predict | `/backup` (archive + signature)     | Next nonce `k`, then the Master Key |
| 4. Forge   | Signing math on our own machine     | A "perfectly valid" `evil.zip`      |
| 5. Escape  | `/restore` + symlink                | Read `/flag` via `/read`            |

---

## 📌 Part 1 — Recon: What Is the Target?

### Machine vs. Webpage
It is **both**.

- **The machine:** a remote computer (a Linux server) hosted in the cloud specifically for this CTF competition.
- **The webpage:** that machine runs a web application. Anyone with the URL can open it in a browser like Chrome or Safari.

### What does it do?
A bare-bones **cloud storage drive** (like a very minimal Google Drive or Dropbox). When you register and log in, the server gives you a private folder on its hard drive. Inside it you can create subfolders and organize your text files.

### How does the user interact with it?
Not only by uploading ZIP files. A normal user interacts through a browser or by sending raw HTTP requests via scripts. The app exposes these routes:

| Route | Purpose |
| :--- | :--- |
| `/register`, `/login` | Create an account, authenticate |
| `/mkdir` | Create a new folder |
| `/backup` | Download your files as a signed archive |
| `/restore` | Upload a signed archive back |
| `/read?path=...` | Read a stored file (the extraction point) |

---

## 📦 Part 2 — The Custom Archive Format

The server doesn't use plain ZIP. On `/backup` it builds the file like a sandwich:

| Section | What it contains | Built by |
| :--- | :--- | :--- |
| **Header** | Metadata about the files + a **CRC** (tamper seal number) | Server |
| **Payload** | Your actual files, compressed together | Server |
| **Footer** | The digital signature `(r, s)` | Server, using its Master Key and a random nonce `k` |

> [!IMPORTANT]
> Both the CRC and the signature depend on the **PRNG**. If the PRNG is MT19937 and we can recover its state, `k` is no longer secret — and neither is the Master Key.

### Backup vs. Restore: who sends what?
- **Server → user (Backup):** user clicks Backup (`/backup`). The server collects their files, bundles them into an archive, signs it, and sends it down as a safety copy.
- **User → server (Restore):** user uploads that exact same signed archive to `/restore`. The server verifies the signature to prove the user didn't modify it, unpacks it, and restores the files.

---

## 🔐 Part 3 — The Signing Math

The archive's payload holds hashed strings, and the footer holds the signature `(r, s)`. The signature is the classic ECDSA/DSA form:

$$s = k^{-1} \times (z + r \times d)$$

Where:
- `z` = the hash of the signed data (obtained by **hashing** the header/payload — you cannot "un-hash" it)
- `r` = part of the signature, derived from `k × G`
- `d` = the server's **Master Private Key**
- `k` = the random nonce, drawn from MT19937

Rearranged, only **two unknowns** remain, `k` and `d`:

$$d = \frac{s \cdot k - z}{r}$$

So the entire scheme collapses the moment we can guess `k`. Since the binary's algorithm is fully mapped out, `k` is simply the **next output of MT19937** — and we can compute that output ourselves.

---

## 🎲 Part 4 — Stealing the PRNG State via Session IDs

The session ID is generated from the same MT19937 stream. So:

1. Log in and out repeatedly, collecting the **session IDs** in order.
2. Each ID is a window into the generator's output (if it's raw state, `untemper` it back; if it's derived, work out the mapping).
3. After enough samples, the full **624 × 32-bit internal state** is reconstructed — every future and past output is now computable.
4. Query `/backup` to burn nonces until the counter lines up, then predict the exact `k` used in the signature you downloaded.
5. Plug `k` and `z` into the formula → **Master Key recovered**.

---

## 🧨 Part 5 — Forging the Malicious Backup

1. **Create the trap.** On your own machine, make a special shortcut file (**symlink**) pointing at `/flag`, the server's ultimate secret file. Pack it into a new archive: `evil.zip`.
2. **Forge the signature.** Normally `/restore`'s gatekeeper rejects `evil.zip` for lacking a valid signature. But we hold the Master Private Key, so a local script can mathematically stamp a **perfect, authentic-looking signature** onto it.
3. **Trigger the exploit.** Upload the signed `evil.zip` through `/restore`.
4. **The server is fooled.** It verifies the signature with its internal math: *"100% match — I must have generated this myself."*
5. **The theft.** It blindly extracts, placing your symlink in your folder. When you then request that file via `/read?path=evil`, the server follows the link, reaches outside the jail cell, and hands you the flag.

> [!TIP]
> The signature check was never the real security boundary — **path containment** was. A valid signature only proves *origin*, not *safety*.

---

## 🧠 Why It Breaks: Three Separate Failures

| #   | Failure                          | Root Cause                                                        |
| :-- | :------------------------------- | :---------------------------------------------------------------- |
| 1   | Nonce predictability             | `k` from MT19937 instead of a CSPRNG (`secrets` / `/dev/urandom`) |
| 2   | Nonce leakage                    | PRNG output exposed through session IDs                           |
| 3   | Arbitrary write / path traversal | `/restore` extracts symlinks without validating the resolved path |

---

## ❓ Open Questions / To Verify

- [ ] Is the "CRC" in the header actually a CRC, or is it itself the signed digest `z`?
- [ ] Which curve is used (P-256? secp256k1?) — affects how `k × G` is checked.
- [ ] Are session IDs raw 32-bit MT outputs, or encoded/truncated? Exact extraction matters.
- [ ] Is the session PRNG the *same* stream as the nonce PRNG, or a separate instance seeded identically?
- [ ] Does `/restore` allow `../` paths directly, or is the symlink genuinely required?

## 🔗 Related

- PRNG / MT19937 state recovery — "untempering" 624 words
- ECDSA nonce reuse & predictable-nonce attacks
- ZIP/tar symlink traversal (Zip Slip family)
