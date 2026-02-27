---
title: "The Past Few Days"
date: 2026-02-27T15:22:34-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The past few days have been getting logs into the logging server.  We have kube logs, openclaw logs, matrix logs (all the backends, db, synapse, web and admin) and caddy logs.  I've got a dashboard that is showing me global web traffic now so I can see any malicious activity and where it's coming from and react.  Such as the gptbot.. it started scraping the copy of these blog pages git repository, every commit and every document in every commit.

I took action and talked with r3r4um.  Told him I wanted this user agent blocked.  He recommended we put it in our robots.txt but maybe he ran into an issue with caddy serving up pages?  I don't know what his issue was, I should have paid more attention but I loved how he solved it.  He created a caddy snippet:

```
# GPTBot blocking snippet
(gptbot_blocker) {
    @gptbot header User-Agent *GPTBot*
    handle @gptbot {
        respond "Forbidden" 403
    }
}
```

Then referenced that in the caddy files:

```
domain.name {
    import gptbot_blocker
    import http_request_logging
    
    # Block signup endpoint
    @signup path /user/sign_up
    handle @signup {
        respond 404
    }

    reverse_proxy http://<backend_ip> {
        header_up Host domain.name
        header_up X-Forwarded-For {http.request.remote.host}
        header_up X-Forwarded-Proto {http.request.scheme}
        header_up X-Forwarded-Host domain.name
        lb_policy random
    }

    encode gzip
}
```

So we're returning this specific bot a 403 telling it it's not authorized to view these pages.  Pretty elegent if you ask me.  I would have gone with the robots.txt but this hard stop is much better.  They just don't even get to see anything so get nothing to crawl.  A buddy gave me a honeypot script I could have stood up a backend for but they had already gotten the hint and stopped even trying.  Alas, I'll be setting that honeypot up though for sure.  I'd love to trap 404's at caddy and then forward to an 'error page' which would be the honeypot url.  We'll see on the next attack, if I don't just turn the port forwarding off ;).

The rest of the time I've just been fixing different things.  The OpenClaw vm just hard locked up.  I went to ask r3r4um what was up with the matrix server since it was down and he didn't respond.  I then remembered that they were on the same vm, lol.  So I pulled up the terminal and sure enough, KDE is just hard stopped, not sure how much life there was in that vm but I coudn't shell in either.  Force off and power back on, hope all is well.

Not much else to report at this time.