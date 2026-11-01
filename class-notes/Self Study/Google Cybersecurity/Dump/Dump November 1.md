# Course 5 Module 2

Public Key Infrastructure (PKI) is an encryption framework designed to secure online communication by managing encryption methods and establishing trust between parties.

Encryption Methods Used in PKI

- Asymmetric encryption utilizes a public key to encrypt data and a private key to decrypt it, offering high security but slower speeds.
- Symmetric encryption uses a single shared secret key for both encryption and decryption, providing faster communication at the expense of lower security.

How PKI Combines Both Types

- Applications often balance speed and security by starting connections with asymmetric encryption before switching to symmetric encryption.
- Both methods face the shared vulnerability of needing a reliable way to verify the identity of the person or system receiving the keys.

Establishing Trust with Digital Certificates

- PKI resolves key management and trust issues through digital certificates, which function as digital ID badges verifying the identity of a public key holder.
- A trusted Certificate Authority (CA) verifies an organization's details, signs the certificate with its own private key, and validates authenticity.
