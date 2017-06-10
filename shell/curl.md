#curl
curl -> c-url (hehe)
Flag -L tells curl to follows the redirects
```bash
curl -L 'http://google.com'
```
will return the complete body returned

```bash
curl -o file_name.txt -L 'http://google.com'
```
will redirect the output to the file named ```file_name.txt```