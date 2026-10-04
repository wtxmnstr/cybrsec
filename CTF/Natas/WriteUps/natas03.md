# Natas3

*	user: `natas3`
*	pass: `K30JrSRHzjxq3paUQuwozY4MNvmNFyhI`
*	url: `http://natas3.natas.labs.overthewire.org`
*	flag: `JDrPnuZAKyl6MkiqQGFIddrqpvgOASth`

## WriteUp
1. Upon entering the source code, we are greeted with the same phrase as in the previous level and see a hint: `"No more information leaks!! Not even Google will find it this time..."`![](screenshots/natas03_code.png)

2. We search online to identify directories not indexed by search engines and discover that this can be achieved by checking the `robots.txt` file. We are trying to read this file. We attempt to read this file and see that there is a hidden directory, `/s3cr3t/`. ![](screenshots/natas03_robots.png)
   
   ***P.S.** We could have used a tool like `ffuf` to find the hidden directory.
2. Open users.txt and you'll see the password. 
![](screenshots/natas03_s3cr3t.png)