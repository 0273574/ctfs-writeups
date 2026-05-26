# UMassCTF 2026 - BrOWSER BOSS FIGHT 
---

## 🔍 Challenge Overview

A Mario-themed web challenge that required bypassing client-side JavaScript, intercepting HTTP traffic with Burp Suite, reading a hint hidden in a response header, and finally forging a cookie to gain access to Bowser's castle and retrieve the flag.

---

## 🧩 Solution Walkthrough

### Step 1 — Opening the Page

The challenge page greeted us with a Mario-style scene — a character standing before a gate with an input field.

![Challenge page]()

Inspecting the page source revealed a suspicious JavaScript snippet tied to the form submission:

```javascript
document.getElementById('key-form').onsubmit = function() {
    const knockOnDoor = document.getElementById('key');
    // It replaces whatever they typed with 'WEAK_NON_KOOPA_KNOCK'
    knockOnDoor.value = "WEAK_NON_KOOPA_KNOCK";
    return true;
};
```

No matter what value we typed into the input, the JS would silently overwrite it with `WEAK_NON_KOOPA_KNOCK` before submission — a classic client-side bypass target.

![Source JS code]()

---

### Step 2 — Submitting the Weak Key

Submitting the form with the replaced value redirected us to `/kamel.html` — a dead end that confirmed the key was wrong.

![kamel.html page]()

---

### Step 3 — Intercepting with Burp Suite

We captured the request and response using **Burp Suite**. The response headers contained a clever in-universe hint:

```
Server: BrOWSERS CASTLE (A note outside: "King Koopa, if you forget the key,
check under_the_doormat! - Sincerely, your faithful servant, Kamek")
```

The `Server` header was telling us to try `under_the_doormat` as the key value.

![Burp Suite intercept]()

---

### Step 4 — Bypassing the JS & Sending the Real Key

Since the JavaScript was replacing our input value, we bypassed it by modifying the request directly in Burp Suite, setting the key to `under_the_doormat`. This redirected us to `/bowsers_castle.html`.

![bowsers_castle.html]()

---

### Step 5 — Analysing the Response Cookies

The response was packed with a large number of `Set-Cookie` headers (themed after Mario enemies), but buried at the bottom was the key detail:

```http
HTTP/1.1 200 OK
X-Powered-By: Express
Server: BrOWSERS CASTLE (...)
Set-Cookie: goomba_guard_1=pq9z4; Path=/
Set-Cookie: koopa_guard_1=6clt1o; Path=/
Set-Cookie: mushroom_huffing_italian_1=v3ydjc; Path=/
Set-Cookie: dry_bones_1=r5flv; Path=/
Set-Cookie: fruit_named_woman_1=oq3jhm; Path=/
...
Set-Cookie: hasAxe=false; Path=/
```

The `hasAxe=false` cookie was the gatekeeper. In Mario lore, the axe is used to defeat Bowser — so setting it to `true` seemed like the right move.

![Full response with cookies]()

---

### Step 6 — Forging the Cookie & Getting the Flag

We pasted the following payload into the browser console to override the cookie and re-fetch the castle page:

```javascript
document.cookie = "hasAxe=true; path=/";
fetch('/bowsers_castle.html').then(r => r.text()).then(console.log);
```

The server trusted the client-side cookie — and returned the flag. 🏆

![Flag]()

---

## 🛠️ Techniques Used

| Technique | Description |
|-----------|-------------|
| **Client-side JS bypass** | JS was overwriting the form input; bypassed via Burp Suite request modification |
| **HTTP header analysis** | The `Server` response header contained a hidden hint with the correct key value |
| **Cookie manipulation** | Forged `hasAxe=true` cookie via browser console to unlock the flag |

---
