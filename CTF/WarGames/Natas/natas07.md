# Natas7

*	user: `natas7`
*	pass: `B1szg95UcTnrzwnF3i3TzYHlyYh8iBV0`
*	url: `http://natas7.natas.labs.overthewire.org`
*	flag: `ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc`

## WriteUp
1. We see two links. When we click on "Home," the `page` parameter returns a file containing "home."
   
   ![](natas07_source.png)

2. If we enter, for example, `page=/etc/passwd`, we get the contents of that file.

   ![](natas07_etcpasswd.png)

3.  A hint from the source code tells us that the password is located in the file `/etc/natas_webpass/natas8`. Let's go there.

 ![](natas07_pass.png)  