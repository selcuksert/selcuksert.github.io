---
layout: post
title:  Single Page Application Security with OAuth and OpenID Connect
categories: [Security,OAuth2,OIDC,SPA]
excerpt: The decoupling of identity source and the application requires a new way to exchange the authentication and authorization information between these systems. An answer to this problem is OAuth which introduces a way to use a digital token as a key to access the APIs exposed by these applications. As OAuth specification mostly tried to address authorization of access requests since day 1 another specification named as OIDC (OpenID Connect) introduced as an authentication layer on top of OAuth.
---
Just like our immune systems are being tested by the novel Coronavirus nowadays, digital channels are being tested in various ways. Getting them and their users securely access to resources is becoming a more and more daunting task. That is why new methodologies/technologies are still emerging besides traditional methods like "basic authentication, form login, certificate authentication, cookie validation".

Today the ease of access to internet and the smart devices paves the way utilizing digital channels increasingly for fulfilling daily needs. Organizations are adding vast amount of business capabilities on them to align with this trend fueling their digital transformation efforts. The prevalence of mobile applications, JavaScript based Single Page Web Applications (SPA) and the technological advancements on platforms (smart devices, browsers, etc.) that host them results in more API (Application Programming Interface)-led solutions and end user clients are being implemented.

![spotify-login](/images/oauth-oidc/spotify.jpg)

Most of us have identities on social media and digital services platforms. These platforms started to act as identity providers that enables users to use their existing identities or to delegate them to access other online services or platforms. The decoupling of identity source and the application requires a new way to exchange the authentication and authorization information between these systems. An answer to this problem is OAuth which introduces a way to use a digital token as a key to access the APIs exposed by these applications. As OAuth specification mostly tried to address authorization of access requests since day 1 another specification named as OIDC (OpenID Connect) introduced as an authentication layer on top of OAuth.
