# Nitrokey instructions 

## Determing which nitrokey is which 

- Install the opensc package (debian):
```
sudo apt-get install opensc
```

- Start the pcsc deamon:
```
pcscd
```

- List the nitrokeys:
```
pkcs11-tool --list-slots
```

## Resetting nitrokey back to factory settings

- Change identity to 0:

```
gpg-connect-agent --hex "scd apdu  00 85 00 00" /bye
```

- Restore communication to the card:

```
gpg --card-status
```

- Confirm the change to identity by locating the 10th byte of the application ID, it should be FE:
```
Reader ...........: 20A0:4211:FSIJ-1.2.19-7AF10C50:0
Application ID ...: D276000124010200FFFE7AF10C500000
Application type .: OpenPGP
Version ..........: 2.0
Manufacturer .....: unmanaged S/N range
Serial number ....: 7AF10C50
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......:
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 3 3
Signature counter : 0
KDF setting ......: off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
```

- Reset card back to factory settings:

```
gpg --card-edit -> admin -> factory-reset
```

- Change identity to 1:

```
gpg-connect-agent --hex "scd apdu  00 85 00 01" /bye
```

- Restore communication to the card:

```
gpg --card-status
```

- Confirm the change to identity by locating the 10th byte of the application ID, it should be 01:

```
Reader ...........: Nitrokey Nitrokey Start (FSIJ-1.2.19-7AF10C50) 00 00
Application ID ...: D276000124010200FF017AF10C500000
Application type .: OpenPGP
Version ..........: 2.0
Manufacturer .....: unmanaged S/N range
Serial number ....: 7AF10C50
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......:
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 3 3
Signature counter : 0
KDF setting ......: off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
```

- Reset card back to factory settings:

```
gpg --card-edit -> admin -> factory-reset
```

- Change identity to 2:

```
gpg-connect-agent --hex "scd apdu  00 85 00 02" /bye
```

- Restore communication to the card:

```
gpg --card-status
```

- Confirm the change to identity by locating the 10th byte of the application ID, it should be 02:
```
Reader ...........: Nitrokey Nitrokey Start (FSIJ-1.2.19-7AF10C50) 00 00
Application ID ...: D276000124010200FF027AF10C500000
Application type .: OpenPGP
Version ..........: 2.0
Manufacturer .....: unmanaged S/N range
Serial number ....: 7AF10C50
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......:
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 3 3
Signature counter : 0
KDF setting ......: off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
```

- Reset card back to factory settings:

```
gpg --card-edit -> admin -> factory-reset
```

## Configuring HSM's

### HSM1

#### Generating keys
- Followed the instructions in:  https://docs.nitrokey.com/nitrokeys/features/openpgp-card/openpgp-keygen-backup

- Enter command "gpg --full-generate-key --expert" to start a guided key generation (the options used are detailed below):

```
gpg --full-generate-key --expert
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 2048
Requested keysize is 2048 bits
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want for the subkey? (3072) 2048
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ApricotHSM1
Email address:
Comment:
You selected this USER-ID:
    "ApricotHSM1"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
Please enter the passphrase to
protect your new key
Passphrase: <passphrase>
Repeat: <passphrase>
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key F15F8DF853D9D1DA marked as ultimately trusted
gpg: revocation certificate stored as '/home/dstorer/.gnupg/openpgp-revocs.d/45BA1026CC346469706F8401F15F8DF853D9D1DA.rev'
public and secret key created and signed.

pub   rsa2048 2025-02-25 [SC]
      45BA1026CC346469706F8401F15F8DF853D9D1DA
uid                      ApricotHSM1
sub   rsa2048 2025-02-25 [E]

```

- Export key to nitrokey using command "gpg --edit-key --expert keyID":

```
gpg --edit-key --expert ApricotHSM1
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   4  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 4u
sec  rsa2048/F15F8DF853D9D1DA
     created: 2025-02-25  expires: never       usage: SC
     trust: ultimate      validity: ultimate
ssb  rsa2048/AA0A18E60AF9AA2B
     created: 2025-02-25  expires: never       usage: E
[ultimate] (1). ApricotHSM1

qgpg> keytocard
Really move the primary key? (y/N) y
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1
Please enter your passphrase, so that the secret key can be unlocked for this session
Passphrase:
Please enter the Admin PIN

Number: FFFE 7AF10C50
Holder:

Remaining attempts: 2
Admin PIN:

sec  rsa2048/F15F8DF853D9D1DA
     created: 2025-02-25  expires: never       usage: SC
     trust: ultimate      validity: ultimate
ssb  rsa2048/AA0A18E60AF9AA2B
     created: 2025-02-25  expires: never       usage: E
[ultimate] (1). ApricotHSM1
```

- Export the encryption subkey to nitrokey:

```
gpg> key 1

sec  rsa2048/F15F8DF853D9D1DA
     created: 2025-02-25  expires: never       usage: SC
     card-no: FFFE 7AF10C50
     trust: ultimate      validity: ultimate
ssb* rsa2048/AA0A18E60AF9AA2B
     created: 2025-02-25  expires: never       usage: E
[ultimate] (1). ApricotHSM1

gpg> keytocard
Please select where to store the key:
   (2) Encryption key
Your selection? 2
Please enter your passphrase, so that the secret key can be unlocked for this session
Passphrase: <passphrase>

sec  rsa2048/F15F8DF853D9D1DA
     created: 2025-02-25  expires: never       usage: SC
     card-no: FFFE 7AF10C50
     trust: ultimate      validity: ultimate
ssb* rsa2048/AA0A18E60AF9AA2B
     created: 2025-02-25  expires: never       usage: E
[ultimate] (1). ApricotHSM1
```

#### Change pins

- Enter passwd settings:
```
gpg –-card-edit -> admin -> passwd
```

- Select 3 for admin pin

- Change admin pin to: <admin_pin>

- Select 1 for user pin

- Change user pin to: <pin>

### HSM3

#### Generating keys
- Followed the instructions in:  https://docs.nitrokey.com/nitrokeys/features/openpgp-card/openpgp-keygen-backup

- Enter command "gpg --full-generate-key --expert" to start a guided key generation (the options used are detailed below):

```
gpg --full-generate-key --expert
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 2048
Requested keysize is 2048 bits
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want for the subkey? (3072) 2048
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ApricotHSM3
Email address: 
Comment: 
You selected this USER-ID:
    "ApricotHSM3"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
Please enter the passphrase to
protect your new key
Passphrase: <passphrase>
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key 0B8FD0D0BA6FD9B8 marked as ultimately trusted
gpg: revocation certificate stored as '/home/dstorer/.gnupg/openpgp-revocs.d/3439D42363E47BBE1AF8B7390B8FD0D0BA6FD9B8.rev'
public and secret key created and signed.

pub   rsa2048 2025-02-24 [SC]
      3439D42363E47BBE1AF8B7390B8FD0D0BA6FD9B8
uid                      ApricotHSM3
sub   rsa2048 2025-02-24 [E]
```

- Export key to nitrokey using command "gpg --edit-key --expert keyID":

```
gpg --edit-key --expert ApricotHSM3
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   5  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 5u
sec  rsa2048/D99D5F3C16D1749C
     created: 2025-02-24  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa2048/E53648ADBE876D90
     created: 2025-02-24  expires: never       usage: E   
[ultimate] (1). ApricotHSM3

gpg> keytocard
Really move the primary key? (y/N) y
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1
Please enter your passphrase, so that the secret key can be unlocked for this session
Passphrase: 
Please enter the Admin PIN

Number: FFFE A42D8256
Holder: 
Admin PIN: 

sec  rsa2048/D99D5F3C16D1749C
     created: 2025-02-24  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa2048/E53648ADBE876D90
     created: 2025-02-24  expires: never       usage: E   
[ultimate] (1). ApricotHSM3
```

- Export the encryption subkey to nitrokey:

```
gpg> key 1

sec  rsa2048/D99D5F3C16D1749C
     created: 2025-02-24  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb* rsa2048/E53648ADBE876D90
     created: 2025-02-24  expires: never       usage: E   
[ultimate] (1). ApricotHSM3

gpg> keytocard
Please select where to store the key:
   (2) Encryption key
Your selection? 2
Please enter your passphrase, so that the secret key can be unlocked for this session
Passphrase: <passphrase>

sec  rsa2048/D99D5F3C16D1749C
     created: 2025-02-24  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb* rsa2048/E53648ADBE876D90
     created: 2025-02-24  expires: never       usage: E   
[ultimate] (1). ApricotHSM3
```

#### Change pins

- Enter passwd settings:
```
gpg –-card-edit -> admin -> passwd
```

- Select 3 for admin pin

- Change admin pin to: <admin_pin>

- Select 1 for user pin

- Change user pin to: <admin_pin>

