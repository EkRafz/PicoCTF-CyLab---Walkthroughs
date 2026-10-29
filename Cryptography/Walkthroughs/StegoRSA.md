> A message has been encrypted using RSA. The public key is gone… but someone might have been careless with the private key. Can you recover it and decrypt the message? Download the [flag](https://challenge-files.cylabacademy.net/library/4c19613e66e3f22baabf929555a826f8ebd997a002b008eb4e51dc2849aca14e/flag.enc) and [image](https://challenge-files.cylabacademy.net/library/4c19613e66e3f22baabf929555a826f8ebd997a002b008eb4e51dc2849aca14e/image.jpg).

---

## Challenge Access

| Field    | Value                                                                                                                                                                                                                                                                                |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category | Cryptography                                                                                                                                                                                                                                                                         |
|          |                                                                                                                                                                                                                                                                                      |
| Files    | [`flag.enc`](https://challenge-files.cylabacademy.net/library/4c19613e66e3f22baabf929555a826f8ebd997a002b008eb4e51dc2849aca14e/flag.enc), [`image.jpg`](https://challenge-files.cylabacademy.net/library/4c19613e66e3f22baabf929555a826f8ebd997a002b008eb4e51dc2849aca14e/image.jpg) |

---

## Commands Used

- `exiftool` → Inspect image metadata
- `nano` → Write the recovered key to a file
- `openssl` → Validate the key and decrypt the flag
- `xxd`

---

## Solution

### Step 1 -- Understand the Problem

We're given two files: an encrypted message (`flag.enc`) and an image (`image.jpg`). The flag is RSA-encrypted and the public key is "gone," but the prompt hints that whoever generated the challenge was careless with the private key. The image is the obvious place to hide it, RSA keys are plain text (PEM format), which can be embedded in any metadata field of an image without affecting how it displays.

---
### Step 2 — Inspect the Image Metadata

```bash
$ exiftool image.jpg 
ExifTool Version Number         : 12.76
File Name                       : image.jpg
Directory                       : .
File Size                       : 21 kB
File Modification Date/Time     : 2026:10:26 09:43:58+01:00
File Access Date/Time           : 2026:10:26 09:45:08+01:00
File Inode Change Date/Time     : 2026:10:26 09:44:00+01:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : 2d2d2d2d2d424547494e2050524956415445204b45592d2d2d2d2d0a4d494945764149424144414e42676b71686b6947397730424151454641415343424b59776767536941674541416f4942415143734d6179734e586530784b45720a3343786a75324d355a384c6a485876684c76767471726c6c376c79794a457639597835613579465a6248727859344f647472636f4f653364416237764638376d0a4b566f4a4b614b4d4472596957377257536b786e732f392b616c52744b6d4a4f767875474f47562b6e453765724d4d766949777830747178596b364b676253790a6f5a5546344c6b58744e676567384363647651464537497555493754325a6b5759552b4c6f2b585279315830336f6c6338614135504e4b6b42747966723476790a64774f7755376f64692b313449394b51584b6c744c306c623264366651554658552b61476c596a64514e53654f3141307237484571514d54717a646d6f5a78390a49766a636473623638514e70612f2f56583169435a6662516d7359336a785443574e306a4d754470733167436b45385365755a5853454874775a4f5337474d620a306b34495944733941674d42414145436767454142566670725958354f78613358554264764b752f646d64462f614d2b664a4b7a2b616249744b556d504b4f2f0a473462766c62575062424a467a35707151454c79394b314f75476c394b4465686e6b33524d79504b485455665179313849784d32396c7572654c5639593834380a6d5655737076754450537973572b457a6b636678526369496b70663971316d6c707233523437314e2f655769703656626e524b51547662537a6356546c3763660a5455645175777255432b5671706b4273774a3231565638594a7061743770584d4e5a6169556b4d43684a766a6d4d4c676c4248506f742b614a634f62724570340a4d735566425748452b63683548786f794f592b6668384c626932585138487965546d476e507044426548763538756e75784d3550676a4e6854556b6e677573350a4e4879623168325569363331635a76342b4a6f53306f397749766b557647306a4a4456534f2f446c74774b4267514471763862456f7855764b6c57515471542b0a484371776d32782f4a3043707032517269626d73784b47626f30424361652b38314e633752764f613173776c6450645352665558507a4c766c6249497134676d0a6656387558794e50595350692b5763457a706d726472472b5a7567566f364e3851334b78327973686c2b47444c755a6577456b77574635594e4d6877512b48370a4b4d4176713532444e524b54596d36744e6c56774e3465534e774b42675143377944554f7a306c42516144754973393347335638634a543735796e4e394e336d0a663051337441633350466d446e3758506a4a48585753766f4450682b31554352332b777973706a794c6d4e7867644a746945346c32476169596336797a33486e0a6b384567577736755249595a514a3546617a477559587453416e6d7866766666692f77735a496e306f2b2b634e587644776e6d456a633046734d6e73314179470a4e532b34397053304b774b4267466b4955304c5139332f6a396e2b3831436273562b53546a68724c73474b6e38663378526c4b5959524b2b582b6a51645053520a5a50412b674c59424d58776a57496f44726d6b6174783758443662714a3845785736416978536d6546685a4873534c4a6d4d374850624b524a51545141336c6c0a75324a70565870774f442b67306736747278522b37346c5575354359726d53726c2f6838686b6a31494135324472507a4834526f43797352416f4741513939780a6a68444f59592f66497a506832534e4c7139626c6773646a444e5a632f6c7378785243464b796e64455035424d4f75574b306848336e415277595a432b48622f0a594c4f394a497776315738664b41674564786c6f763369717857624676505a69373962357a334e6c4a4f742b53596b64512f4a6f4b4a524d65434e305a4a43730a6254394a7665736a534572736d53314a7a7565534732527a462b37744e2f5578637146747a6c4543675942574936336b654b58506236365544655034363546470a7466756159435030526b5a776453652b737a644b396e587a2f50634b42663053342f6977746d4b42676f384d50363059572b797777302f73356a534c344450680a446834316f4b43373738475837374a4f625638534f486b44557a6255456d4e7252694b6f4a3774674c62343042536e663555515338496c6d3873792f38376c440a41624f3259623558477056344641674b4e6d75737a773d3d0a2d2d2d2d2d454e442050524956415445204b45592d2d2d2d2d0a
Image Width                     : 512
Image Height                    : 512
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 512x512
Megapixels                      : 0.262
```

Note that the `comment` has the private key in hex and not in ASCII

---
### Step 3 -- Decode the Hex to ASCII

To decode the Hex to ASCII, you can use [cyberchef](https://gchq.github.io/CyberChef/) or use can use this command:

``` Shell
$ xxd -r -p chiave 
-----BEGIN PRIVATE KEY-----
MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQCsMaysNXe0xKEr
3Cxju2M5Z8LjHXvhLvvtqrll7lyyJEv9Yx5a5yFZbHrxY4OdtrcoOe3dAb7vF87m
KVoJKaKMDrYiW7rWSkxns/9+alRtKmJOvxuGOGV+nE7erMMviIwx0tqxYk6KgbSy
oZUF4LkXtNgeg8CcdvQFE7IuUI7T2ZkWYU+Lo+XRy1X03olc8aA5PNKkBtyfr4vy
dwOwU7odi+14I9KQXKltL0lb2d6fQUFXU+aGlYjdQNSeO1A0r7HEqQMTqzdmoZx9
Ivjcdsb68QNpa//VX1iCZfbQmsY3jxTCWN0jMuDps1gCkE8SeuZXSEHtwZOS7GMb
0k4IYDs9AgMBAAECggEABVfprYX5Oxa3XUBdvKu/dmdF/aM+fJKz+abItKUmPKO/
G4bvlbWPbBJFz5pqQELy9K1OuGl9KDehnk3RMyPKHTUfQy18IxM29lureLV9Y848
mVUspvuDPSysW+EzkcfxRciIkpf9q1mlpr3R471N/eWip6VbnRKQTvbSzcVTl7cf
TUdQuwrUC+VqpkBswJ21VV8YJpat7pXMNZaiUkMChJvjmMLglBHPot+aJcObrEp4
MsUfBWHE+ch5HxoyOY+fh8Lbi2XQ8HyeTmGnPpDBeHv58unuxM5PgjNhTUkngus5
NHyb1h2Ui631cZv4+JoS0o9wIvkUvG0jJDVSO/DltwKBgQDqv8bEoxUvKlWQTqT+
HCqwm2x/J0Cpp2QribmsxKGbo0BCae+81Nc7RvOa1swldPdSRfUXPzLvlbIIq4gm
fV8uXyNPYSPi+WcEzpmrdrG+ZugVo6N8Q3Kx2yshl+GDLuZewEkwWF5YNMhwQ+H7
KMAvq52DNRKTYm6tNlVwN4eSNwKBgQC7yDUOz0lBQaDuIs93G3V8cJT75ynN9N3m
f0Q3tAc3PFmDn7XPjJHXWSvoDPh+1UCR3+wyspjyLmNxgdJtiE4l2GaiYc6yz3Hn
k8EgWw6uRIYZQJ5FazGuYXtSAnmxfvffi/wsZIn0o++cNXvDwnmEjc0FsMns1AyG
NS+49pS0KwKBgFkIU0LQ93/j9n+81CbsV+STjhrLsGKn8f3xRlKYYRK+X+jQdPSR
ZPA+gLYBMXwjWIoDrmkatx7XD6bqJ8ExW6AixSmeFhZHsSLJmM7HPbKRJQTQA3ll
u2JpVXpwOD+g0g6trxR+74lUu5CYrmSrl/h8hkj1IA52DrPzH4RoCysRAoGAQ99x
jhDOYY/fIzPh2SNLq9blgsdjDNZc/lsxxRCFKyndEP5BMOuWK0hH3nARwYZC+Hb/
YLO9JIwv1W8fKAgEdxlov3iqxWbFvPZi79b5z3NlJOt+SYkdQ/JoKJRMeCN0ZJCs
bT9JvesjSErsmS1JzueSG2RzF+7tN/UxcqFtzlECgYBWI63keKXPb66UDeP465FG
tfuaYCP0RkZwdSe+szdK9nXz/PcKBf0S4/iwtmKBgo8MP60YW+yww0/s5jSL4DPh
Dh41oKC778GX77JObV8SOHkDUzbUEmNrRiKoJ7tgLb40BSnf5UQS8Ilm8sy/87lD
AbO2Yb5XGpV4FAgKNmuszw==
-----END PRIVATE KEY----- 
```

> I saved the hex string from the Comment field into a file called `chiave`

We used `xxd` to convert the hex dump to its original form. Let me explain the options:
- `-r` → converts hex dump
- `-p` → outputs the result
Also all of the info is on the `man pages` of this command, just:ù

``` Shell
$ man xxd
XXD(1)                      General Commands Manual                     XXD(1)

NAME
       xxd - make a hex dump or do the reverse.

SYNOPSIS
       xxd -h[elp]
       xxd [options] [infile [outfile]]
       xxd -r[evert] [options] [infile [outfile]]

										...

-r | -revert
              Reverse  operation: convert (or patch) hex dump into binary.  If
              not writing to stdout, xxd writes into its output  file  without
              truncating it. Use the combination -r -p to read plain hexadeci‐
              mal dumps without line number information and without a particu‐
              lar column layout. Additional whitespace and line breaks are al‐
              lowed  anywhere.  Use  the combination -r -b to read a bits dump
              instead of a hex dump.

										...

-p | -ps | -postscript | -plain
              Output in PostScript continuous hex dump style.  Also  known  as
              plain hex dump style.

```

---
### Step 3 -- Save the Recovered Key

```bash
nano key.pem
```

Paste the the key block exactly as found (headers and footers included), save, and exit.

---
### Step 4 -- Validate the Key

```bash
$ openssl rsa -in key.pem -check
RSA key ok
writing RSA key
-----BEGIN PRIVATE KEY-----
...
...
...
-----END PRIVATE KEY-----
```

`Openssl` is the command-line tool for the OpenSSL cryptography library. It’s a single binary that bundles dozens of subcommands for working with keys, certificates, and ciphers.

``` Shell
OPENSSL(1SSL)                       OpenSSL                      OPENSSL(1SSL)

NAME
       openssl - OpenSSL command line program

SYNOPSIS
       openssl command [ options ... ] [ parameters ... ]

       openssl no-XXX [ options ]

DESCRIPTION
       OpenSSL is a cryptography toolkit implementing the Secure Sockets Layer
       (SSL v2/v3) and Transport Layer Security (TLS v1) network protocols and
       related cryptography standards required by them.

       The openssl program is a command line program for using the various
       cryptography functions of OpenSSL's crypto library from the shell.  It
       can be used for

        o  Creation and management of private keys, public keys and parameters
        o  Public key cryptographic operations
        o  Creation of X.509 certificates, CSRs and CRLs
        o  Calculation of Message Digests and Message Authentication Codes
        o  Encryption and Decryption with Ciphers
        o  SSL/TLS Client and Server Tests
        o  Handling of S/MIME signed or encrypted mail
        o  Timestamp requests, generation and verification
        
								...
								...
								...

		rsa RSA key management.
```

`rsa` is one of the subcommands of `openssl`. It refers to the **Rivest–Shamir–Adleman** asymmetric public-key cryptosystem used for secure key exchange, encryption, and digital signatures. It is designed to **process RSA keys**.
Breaking down the options:
- `-in flag.enc` → the input file containing the ciphertext to decrypt
- `-check` → This catches a key that looks structurally valid (correct PEM headers, parses without error)

This confirms the key parses correctly and is a genuine RSA private key before trying to decrypt anything with it.

---
### Step 5 -- Decrypt the Flag

```bash
$ openssl pkeyutl -decrypt -inkey key.pem -in flag.enc
```

Like `rsa`, `pkeyutl` is one of the subcommands of `openssl`.

``` Shell
OPENSSL(1SSL)                       OpenSSL                      OPENSSL(1SSL)

		pkeyutl
           Public key algorithm cryptographic operation command.
        
```

`pkeyutl` works with any key type (RSA, EC, Ed25519, etc.) and the operation it performs depends on the flags passed.
Breaking down the options:
- `-decrypt` → tells `pkeyutl` to perform a decryption operation rather than encryption, signing, or verification
- `-inkey key.pem` → specifies the key file to use for the operation; since `key.pem` is a private key, this allows decryption of data that was encrypted with the matching public key
- `-in flag.enc` → the input file containing the ciphertext to decrypt

``` Shell
academy{...}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

Now that we have the flag, you can use it to complete the challenge.

---

## Key Takeaways

- **Always validate a key before trusting it** → `openssl rsa -check -noout` confirms the decoded PEM actually parses as a valid key, catching a bad decode (wrong encoding, truncation, stray whitespace) before you waste time debugging a failed `openssl` decrypt.
- **Remember to read man pages** → man pages or any type of documentation is extremely helpful, as it allows us to have the full reasoning behind the command and its option.
- **RSA private keys are self-contained.** A recovered private key (modulus + private exponent) is enough to decrypt on its own, there’s no need to reconstruct the “lost” public key.
