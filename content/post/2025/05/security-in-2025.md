---
title: "Security in 2025"
date: 2025-05-04T00:53:41-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Back years and years ago—circa 2008—we were still figuring out this whole internet thing. Back then, we didn’t have the security tools we do now. 2FA was a luxury, CAPTCHA was just starting to show up, and securing an email server meant more than just relying on MX records. It was a simpler time, but also a less secure one.

Fast forward to 2025, and the landscape has changed dramatically. Today’s web servers don’t just need SSL; they need robust security frameworks that handle everything from authentication to data encryption. Tools like Certbot have simplified the process of obtaining and managing SSL/TLS certificates, allowing even non-technical users to secure their web servers with ease.

In 2025, securing your web server isn’t just about adding an SSL certificate. It’s about implementing a strong PKI (Public Key Infrastructure) that ensures every component of your system is authenticated and encrypted. This is where tools like Certbot shine—they handle the heavy lifting so you don’t have to worry about the underlying complexities.

The evolution from manual certificate management to automated tools like Certbot is a perfect example of how far web security has come. It’s also a reminder that while the tools may have evolved, the fundamental principles of security—like protecting data and ensuring authenticity—remain as important as ever.

In 2025, however, we have other tools at play. How does this intersect with containerized infrastructure like Docker Compose? Let me illustrate with my latest project: it comprises four components—the UI, the Webserver, the JavaScript authentication backend, and the Java data backend. The Webserver serves dual roles: it proxies requests to the two backends while ensuring the UI communicates with the same host it originated from, and it also serves static content for the UI. This Webserver represents our primary security target.

Since we're in containers, and potentially run through kubernetes or some other container orchestrator, we need to take special considerations.  For instance, we can't leave out the ssl config in nginx and boot up for the challenge with Let's Encrypt then spool down, apply ssl config and fire back up.  So, we need to have ssl configured from the get go... which means we need a certificate.  Sort of a chicken and egg scenario.  I didn't even think of this but someone on [medium](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71) did and wrote a nice piece on how to solve it.

Happy reading and make sure to put certs in these containers.