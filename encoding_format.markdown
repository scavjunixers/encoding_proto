# Encoding Format v1.0 #

## Purpose of the document ##

This document defines the structure used to encode the decryption packet
of the secret protocol.

## Abbreviations and Notations ##

Abbreviation | Explanation
-------------:-------------------------
RFU          | Reserved for Future Use
TLV          | Type length value
AES          | Advanced encryption standard
KDF          | Key derivation function
IV           | Initialization vector

## Introduction ##

The encoding format of the secret protocol uses the TLV data structure. It
is made in a self unpacking way such that no external data is needed to
decrypt the final message.

## Packing format ##

External packet:

Tag  | Length |  Name
-----:--------:-------------------------------
'81' |  1-n   |  Wrapper of parameters
     |        |  Tag  |  Length |  Name
     |        |  -----:---------:------------
     |        |  '82' |  1      |  Algorithm
     |        |  '83' |  1      |  Text encoding
     |        |  '84' |  1-n    |  Parameters
     |        |       |         |  Tag  |  Length |  Name
     |        |       |         |  -----:---------:-----------
     |        |       |         |  '80' |  1      |  Block size
     |        |       |         |  '81' |  1      |  Use padding (1: yes, 0: no)
     |        |       |         |  '82' |  1-n    |  IV
     |        |       |         |  '83' |  1-n    |  Key
     |        |       |         |  '84' |  1      |  derivation scheme (optional)
     |        |       |         |  '85' |  1-n    |  salt for kdf (optional)
     |        |       |         |  '86' |  1      |  counter for kdf (optional)
     |        |       |         |  '87' |  1-n    |  passphrase (optional)
     |        |  '85' |  1-n    |  Message

Possible values for algorithm:

---:----
01 | DES
02 | AES
03 | RC4
04 | CAST5
05 | PGP
06 | RSA
07 | EC

Possible derivation schemes:

---:----
01 | Password based derivation function (pbkdf2)
02 | RFU
03 | RFU


Possible values for text encoding:

---:----
01 | ASCII
02 | UTF-8
03 | UTF-16
04 | UCS4

