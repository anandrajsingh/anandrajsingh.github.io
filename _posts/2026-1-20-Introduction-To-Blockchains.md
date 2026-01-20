Some pre-requisites

<p style="font-size:24px;"><b>Encodings</b></p>

**Ascii**
Every byte corresponsds to a text on the computer.

**Hex**
A single hex character can be any of the 16 possible values: <b>0-9</b> and <b>A-F</b>

**Base 64**
Base64 encoding uses 64 different characters (A-Z, a-z, 0-9, +, /). which means each character represent one of 64 possible values.

**Base 58**
It is similar to Base64 but uses a different set of characters to avoid visually similar characters and to make encoded output more user-friendly

Base58 uses 58 different characters (A-Z (excluding I and O), a-z(excluding l), 1-9(excluding 0), +, /)

<p style="font-size:24px;"><b>Hashing</b></p>

Hashing is process of converting data (like a file or a message) into a fixed size string of characters, which typically appears random.

hello -----> hashing algo -----> asd912kdas9cak1ok123j9dkao1

Common hasing algorithms are SHA-256 and MD5

**MD5**
- Output size: 128 bits (32 hex characters)
- Speed: Very fast
- Security: Broken
- Collisions: Easy to generate (two different inputs with same hash)
- Use today: Not in cryptography, not in password hashing, Only for non-security uses (e.g. quick checksums)
- Status: Obselete

**SHA-56**
- Output Size: 256 bits (64 hex characters)
- Speed: Slower than MD5 (but still fast)
- Security: strong
- Collisions: Computationaly infisible with current technology
- Use Today: Cryptography, Digital Signature, Blockchains


<p style="font-size:24px;"><b>Encryption</b></p>
Encryption is the process of converting plain text into unreadable format, called cyphertext, using a specific algorithm and a key. The data can be decrypted back to its original form only with the appropriate key.

**Key Characterstics**
- Reversible: With correct key, cyphertext can be decrypted back to plaintext.
- Key-dependent: The security of encryption relies on the secrecy of the encryption key.

**Two Main types**
- <b>Symetric Encryption</b>: The same key is used for both encryption and decryption.
    hello -----> Encryption (with key) -----> lskdglishglisjsdlkgfjsreoij -----> Decryption (with key) -----> hello
- <b>Asymetric Encryption</b>: Different keys are used for encryption (public key) and decryption (private key)

<p style="font-size:24px;"><b>Asymetric Encryption</b></p>
Asymetric encryption also known as public-key cryptography, is a type of encryption that uses a pair of keys: a public key and a private key. The keys are mathematically related, but it is computationally infeasible to derive private key from public key.

- <b>Public key</b>: The public key a string that can be shared openly.
- <b>Private key</b>: The private key is a secret cryptographic code that must be kept confidential. It is used to decrypt data encrypted with the corresponding pbulic key or to create digital signatures.

**Common Asymetric Encryption Algorithms**
- RSA: Rivest-Shamir-Adleman
- ECDSA: Elliptic Curve Digital Signature Algorithm (ETH and BTC)
- EdDSA: Edwards-curve Digital Signature Algorithm (SOL)

### Common eleptic curves

1. secp256k1 - BTC and ETH
2. ed25519 - SOL

**Few usecases of public key cryptography -** 

1. SSL/TLS certificates
2. SSH keys to connect to servers/push to github
3. Blockchains and cryptocurrencies


<p style="font-size:24px;"><b>Different ways to create public/private keypair</b></p>

### **EdDSA -** Edwards-curve Digital Signature Algorithm  - ED25519

- Using `@noble/ed25519`
    
    ```jsx
    import * as ed from "@noble/ed25519";
    
    async function main() {
      // Generate a secure random private key
      const privKey = ed.utils.randomPrivateKey();
    
      // Convert the message "hello world" to a Uint8Array
      const message = new TextEncoder().encode("hello world");
    
      // Generate the public key from the private key
      const pubKey = await ed.getPublicKeyAsync(privKey);
    
      // Sign the message
      const signature = await ed.signAsync(message, privKey);
    
      // Verify the signature
      const isValid = await ed.verifyAsync(signature, message, pubKey);
    
      // Output the result
      console.log(isValid); // Should print `true` if the signature is valid
    }
    
    main();
    
    ```
    
- Using `@solana/web3.js`
    
    ```jsx
    import { Keypair } from "@solana/web3.js";
    import nacl from "tweetnacl";
    
    // Generate a new keypair
    const keypair = Keypair.generate();
    
    // Extract the public and private keys
    const publicKey = keypair.publicKey.toString();
    const secretKey = keypair.secretKey;
    
    // Display the keys
    console.log("Public Key:", publicKey);
    console.log("Private Key (Secret Key):", secretKey);
    
    // Convert the message "hello world" to a Uint8Array
    const message = new TextEncoder().encode("hello world");
    
    const signature = nacl.sign.detached(message, secretKey);
    const result = nacl.sign.detached.verify(
      message,
      signature,
      keypair.publicKey.toBytes(),
    );
    
    console.log(result);
    ```
    

### **ECDSA (Elliptic Curve Digital Signature Algorithm) - secp256k1**

- Using `@noble/secp256k1`
    
    ```jsx
    import * as secp from "@noble/secp256k1";
    
    async function main() {
      const privKey = secp.utils.randomPrivateKey(); // Secure random private key
      // sha256 of 'hello world'
      const msgHash =
        "b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9";
      const pubKey = secp.getPublicKey(privKey);
      const signature = await secp.signAsync(msgHash, privKey); // Sync methods below
      const isValid = secp.verify(signature, msgHash, pubKey);
      console.log(isValid);
    }
    
    main();
    
    ```
    
- Using `ethers`
    
    ```jsx
    import { ethers } from "ethers";
    
    // Generate a random wallet
    const wallet = ethers.Wallet.createRandom();
    
    // Extract the public and private keys
    const publicKey = wallet.address;
    const privateKey = wallet.privateKey;
    
    console.log("Public Key (Address):", publicKey);
    console.log("Private Key:", privateKey);
    
    // Message to sign
    const message = "hello world";
    
    // Sign the message using the wallet's private key
    const signature = await wallet.signMessage(message);
    console.log("Signature:", signature);
    
    // Verify the signature
    const recoveredAddress = ethers.verifyMessage(message, signature);
    
    console.log("Recovered Address:", recoveredAddress);
    console.log("Signature is valid:", recoveredAddress === publicKey);
    ```

### How would you create a decentralized blockchain/currency?

1. Write a program (./bitcoin_miner.exe) that people can run on their machine to become `miners` of the network
2. Write code in the program that dictates how the currency is minted (new currency is created) and who gets the initial supply (developers, initial investors, airdrop)
3. Let anyone join the network/be a miner by running the program (bitcoin_miner.exe)
4. If people want to send BTC/receive BTC, they should create a `public private keypair`  (no username password like banks)
5. To send BTC from your wallet (your public key) to a different wallet, `sign a transaction/message` and send the transaction to one/many miners (btc-miner-india.100xdevs.com)
6. Miners bundle multiple transactions (lets say all txns that came in the last 10s) in a block. The block is transmitted by the miner to everyone else in the network. This creates a permanent record/ledger of all the blocks since the starting (genesis). 
7. A chain of these blocks is called the blockchain.