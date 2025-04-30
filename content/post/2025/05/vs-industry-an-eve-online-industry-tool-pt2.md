---
title: "VS-Industry: An EVE Online Industry Tool Pt2"
date: 2025-05-01T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I recently heard about the craze of Vibe Coding. I've been using GitHub's Copilot for a while now, and I find it useful, especially for generating the little bit of boilerplate I still need today (I'm looking at you, DTOs). However, I never considered letting GPT write my code completely. While I enjoy coding, I enjoy tech even more, and I had to see this in action. So, I downloaded Cursor.

[Cursor](https://www.cursor.com/) is a company that produces a VS Code forked IDE with an AI Agent. They offer a 14-day free trial, which I knew I wanted to use. This seemed perfect. While my frontend skills are terrible, I know what I want, and I was confident I could describe it to an AI to code it. Plus, I had plenty of data to feed it since my backend was already written. With so much to work with, I dove in.

From the very beginning, I was impressed. I started with a blank folder and asked Cursor to generate a web app scaffolding for the most popular web framework and tooling. It gave me several options, and I opted for React, Vite, and Material UI, as that combination sounded good. React and Material UI weren't new to me, but Vite was. Curious about what it offered beyond Webpack, I decided to give it a try. Cursor generated the scaffolding beautifully. The code and JSON were properly formatted to my taste, and I could easily understand it. I was even more impressed.

I needed to refresh my tokens. My backend implementation for the AuthenticationManager (I think that's what it was called) didn't send refresh requests—ever. Logging in repeatedly wasn't an option, so I searched for a library to handle token refreshes. Of course, I found one written in JavaScript. While I'm not thrilled about JavaScript everywhere, it looked promising and wasn't too outdated. I asked Cursor to generate an authentication backend using this library. It struggled initially to include and transpile the library with the index.ts it generated. After several attempts, it decided to fork the library and translate it into TypeScript, which it did flawlessly.

Vite includes a proxy, allowing the app to behave like it would in a production-like environment. I configured it to send authentication requests to /js-api/ and backend requests to /api/. In hindsight, I might rename /js-api/ to /auth/, but for now, it works. With this setup, I needed to test it. Time for more vibe coding! I asked Cursor to use the js-api backend to get an authentication token, specifying that this was an OAuth interaction. It knew exactly what to do, and the Vite proxy worked as expected.

Now, I could refresh tokens without constantly logging in. I then tweaked the backend to remove authentication handling and instead use the token passed from the frontend. I wrote a servlet filter and leveraged the token storage underpinnings I had built earlier. I introduced a simple JwtHolder to use a thread-local variable for keeping the token in memory. It was straightforward, and tokens were successfully making their way to the backend, with the frontend refreshing them as needed.

At this point, I was thoroughly impressed with Cursor. Not only did it recognize the outdated library and translate it into TypeScript, but it also built the authentication backend. This backend consisted of a few components: an endpoint to get the login URL, a callback handler, a refresh handler, and a /me handler I personally wrote to return a character name and ID for the frontend to load an avatar. Super impressive.

The next step was generating all the pages I needed: an overview page, a warehouse page, an item setup page, an ignored product page, a user page, and more. This process took about eight days, leaving me with six days on my trial when the UI was mostly done. With time left, I decided to focus on refactoring.

I wanted to see if the AI could handle basic updates and refactorings. I started by snapping pictures of the UI and asking it to make specific changes to the elements. Generally, it succeeded on the first try. One significant task was creating a "you're not allowed" page, which I called the SPAIError page. This required checking if there were any users in the system. If not, it would display a page to add the first admin user. After logging in, the app would check with the Java backend to see if the user was allowed access. If not, it would show the SPAIError page. I provided a detailed description and broke the task up into chunks for Cursor which handled it flawlessly.

So, that was my foray into Cursor. The entire frontend, minus small tweaks, was written by an AI.