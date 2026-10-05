# Natas11

*	user: `natas11`
*	pass: `VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd`
*	url: `http://natas11.natas.labs.overthewire.org`
*	flag: `EAGkE8uzFTxeoTT2mMst9Xy7PX6guEng`
## WriteUp

1. In this code snippet, we see that XOR encryption is being used. We see the `$data` array, which is created by the `LoadData` function, with `$defaultdata` passed as a parameter. The key is not present in the code, so we will need to find it.

   ![](screenshots/natas11_XOR.png)

   ![](screenshots/natas11_data.png)

   ![](screenshots/natas11_def_data.png)

2. We also see a code snippet like this at the end. It becomes clear that to extract the password, we need to ensure the `$data` array has the value `"showpassword"=>"yes", "bgcolor"=>"#ffffff"`.

   ![](screenshots/natas11_target.png)

3. Here we see that the `LoadData` function loads the `$data` value from a cookie, encodes it in base64, encrypts it using XOR, and decodes it from JSON.
   ![](screenshots/natas11_LoadData.png)


4. So, to obtain the key, we need to take the data from the cookie and base64-decode it, encode `$defaultdata` as JSON, and XOR these two strings.

5. We know that the cookies aren't exactly Base64, so we need to convert them to the correct format. To do this, I used a random site I found on Google. I could have done it manually, but I couldn't be bothered.
   ![](screenshots/natas11_cookie_json.png)


6. We are slightly modifying the source code, simply making it perform the reverse operation on our `cookie` to extract the key.

   ![](screenshots/natas11_key.png)


7. We can see that the key is cyclic—`"kBSw"`—which means that by encoding `array("showpassword" => "yes", "bgcolor"=>"#ffffff")` using the same algorithm, we will obtain the required cookies.

![](screenshots/natas11_encrypt_cookie.png)

8. Next, simply paste the cookies and refresh the page.

   ![](screenshots/natas11_pass.png)