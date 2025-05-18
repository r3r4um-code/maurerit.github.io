---
title: "VS Industry and Legacy Requirements"
date: 2025-05-19T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I have a couple of requirements in the Java backend that I'll call its legacy requirements. The first is a character name and the second is the corporation ID. These should be retrievable through the token that was given to the backend. These requirements came from the fact that I wasn't handling the token, Spring was. So I needed a way to query for the token, this is where the character name requirement came from. The corporation ID comes from the fact that I was making as few API calls as possible to speed up dev work. To get the corporation ID, you have to make an API call to the public character data endpoint.

I need to refactor the token handling. I have the persistent store that was put in place to save the token after Spring fetched it. I'm now using the token repository. There are all the properties in the main application.yml that need to go. It was fun while it lasted using the out-of-the-box Spring support, but I went in a different direction.

The clients still need the corporation ID, but that's easy enough to refactor so that they accept it as a parameter, and then the service layer can figure out what the corp ID is. I'll more than likely refactor the token storage to also store the corporation ID and then lookup the corp on token receipt. Seems like a good plan, I'll do that before I make another release.

Back into code!