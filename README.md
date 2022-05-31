# debianopenssl
Private keys vulnerable to Debian OpenSSL bug (CVE-2008-0166)

In 2008 a bug in Debian's and Ubuntu's OpenSSL package led to
predictable private keys. While the number of keys is limited,
it takes a few considerations to create a proper list of all
plausibly affected keys. This repository contains the keys used
for the blocklist in the [badkeys](https://badkeys.info) tool.

Notes about the Debian OpenSSL bug:

* The keys depend on the process ID (PID. Valid PIDS range from 0
  to 32767. A key created with PID 0 is unlikely, but they are
  included. On 64 bit plattforms it is possible to configure the
  kernel to allow larger PIDs, but this is unlikely.
* Keys from the openssl command line tool and OpenSSH's ssh-keygen
  are different.
* Keys generated with openssl's genrsa and req commands are the
  same.
* openssl creates different keys depending whether a file .rnd is
  present or absent in the home dir.
* Without the .rnd file older (e.g. 0.9.8c from etch) and newer
  (e.g. 0.9.8g from lenny) openssl versions produce different keys.
* ssh-keygen does not create 512 bit RSA keys (768 is the minimum
  size).
* openssl supports changing the exponent value to 3, but this does
  not change the modulus. Therefore only keys with the default
  exponent value are included, when creating a blocklist it is
  therefore recommended to check the modulus and not the full key.
* ssh-keygen only supports DSA keys with 1024 bit size.
* Generating DSA keys with openssl is a two step process (first
  generating parameters, then the key), therefore the number of
  possible keys is much larger. They are not considered here as
  using DSA keys in SSL/TLS was never common.
* Keys created on the big endian architectures PowerPC, SPARC
  and HPPA are identical.
* Keys created on MIPS are different, however given the rare use
  of that plattform they are not considered.

This repo currently includes:
* RSA keys with 1024, 2048, 3072 and 4096 bit (FIXME: some be32/ssh
  keys missing, will be added soon)
* DSA keys with 1024 bit (only openssh)
* Created with PIDs from 0 to 32767
* Keys created with both openssl and ssh-keygen (dirs ssl/ssh)
* Keys created on little endian 32 bit (x86) and 64 bit (amd64)
  architectures and common big endian 32 bit (PowerPC/SPARC/HPPA)
  architectures.
* All three openssl variations (with/without .rnd, old/new versions)

It does not include unusual key sizes, nonstandard PID values,
nonstandard exponent values, keys created on MIPS, DSA keys created
with openssl.

The directory structure is:
```
rsa2048
 ssl
  le32
   $pid-rnd.key
   $pid-nornd-old.key
   $pid-nornd-new.key
  le64...
  be32...
 ssh
  le32
   $pid.key
  le64...
  be32...
rsa...
dsa1024
 openssh
```

links
=====

* [Debian Wiki page about this bug](https://wiki.debian.org/SSLkeys)
* [Tools and scripts to create the keys](https://github.com/badkeys/debianssltools)
* [When Private Keys are Public, research paper about this bug published in 2009](https://hovav.net/ucsd/papers/yrses09.html)
* [key_generator](https://github.com/CVE-2008-0166/key_generator) and [private_keys](https://github.com/CVE-2008-0166/private_keys) created by Rob Stradling (openssl only)
* [Mirror of old tools/keys by HD Moore](https://github.com/g0tmi1k/debian-ssh)
