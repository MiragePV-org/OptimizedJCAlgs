# Smart Card OpenPGP

## Project Status
- [x] Code is working
- [x] Code is optimized
- [x] Code is beautified
- [ ] Usage info is provided
- [ ] Example is provided
- [ ] Profiling data are collected & interpreted
- [ ] Side-channel vulnerability data are collected
- [x] Diploma thesis article is written

## Briefly about OpenPGP
OpenPGP is an open-source implementation of the PGP protocol. PGP allows users to securely send and receive e-mails. GnuPG offers a possibility to use smart cards to store and use cryptographic keys. Smart Card OpenPGP implements an applet that is compatible with GnuPG.

## Usage
The complete usage instructions can be found on GnuPG page:
https://www.gnupg.org/howtos/card-howto/en/ch01.html

To use this applet with GnuPG, use this PKG and AID when building a .cap file: /br
PKG: D27600012401 /br
AID: D2760001240102000000000000010000

## Example
![OpenPGP on card](https://is.muni.cz/www/kewo/GPG_CLI_blur.png?1542469599284)

## Optimizations used
* Inlined single-use private methods
* Minimized number of method parameters

## Performance measurement results