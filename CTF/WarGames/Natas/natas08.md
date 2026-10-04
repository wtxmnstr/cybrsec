# Natas8

*	user: `natas8`
*	pass: `ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc`
*	url: `http://natas8.natas.labs.overthewire.org`
*	flag: `UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9`

## WriteUp
1. At this level, we are given the PHP source code. We see that the secret is Base64-encoded; then, the `strrev` function reverses it, and it is subsequently converted to hex.

   
   ![](natas08_source.png)

2. We simply need to perform the reverse action. `xxd -r` converts the hex back. `rev` reverses the result. And then we decode the base64.
   
![](natas08_decode.png)

![](natas08_pass.png)