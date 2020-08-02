## Challenge : Buggy

### Solution

The challenge is about bypassing 403 error along with file upload vulneribility and LFI.<br>
When we entered the website there's a login form and a link to register.
After clicking on register we see a ```403 Forbidden``` error.

![error](/Buggy/imgs/403.jpg)

Looking at this, thought we can bypass it which is smuggling the request to get registered. After some googling i found a way to bypass it ```/./register.php?username=ph03n1x&password=ph03n1x```<br>
Intercepting the request with burp and forwarding it, our registration is successful.

![burp](/Buggy/imgs/Request.jpg)

Logging in, we find a file uploader which accepts any file extension. Uploading the file, it removes the uploaded file extension and convert the file name to a hash.
I thought of getting a PHP Shell and uploaded a php file.<br>
After uploading the php, there's an option to ```include``` the file. We get a page with following details
> The file has been uploaded to: /var/www/uploads/df0f1a1ac715de9266c8d8391769156a
>
> To include the file, use ?include=

Here we get ```LFI``` at ```/../../../var/www/uploads/df0f1a1ac715de9266c8d8391769156a```
This ```include``` option allows only the content of `http (or) https`.<br>
The final payload for LFI will be `/index.php?include=http:/../../../var/www/uploads/df0f1a1ac715de9266c8d8391769156a`<br>
After getting the shell, `cat` the files in directory.<br>
The flag is in `config.php`.

### Flag: `inctf{REQU357_5MUGG71NG_4ND_1NCLUD3_F0R_TH3_W1N}`



