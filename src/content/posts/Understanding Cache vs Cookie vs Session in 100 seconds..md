---
title: Understanding Cache vs Cookie vs Session in 100 seconds.
pubDate: 2025-04-13
categories: ['Dev']
description: ''
slug: dev
---

# Table of Contents
- [At the beginning](#at-the-beginning)
- [What is Cache vs Cookie vs Session?](#what-is-cache-vs-cookie-vs-session)
- [How do they work?](#how-do-they-work)
- [Conclusion](#conclusion)

# At the beginning

Have you tried to figure out the difference between Cache vs Cookies vs Session? The operations of these three objects are quite similar. However, what precisely are they, what issues are they addressing, and how are they unique? In this article, it makes you understand those differences much more clearly for sure.

# What is Cache vs Cookie vs Session?

## Cache
Cache is a temporary storage used by web browsers or web servers to store frequently accessed data. The purpose of cache is to improve performance by reducing the time needed to fetch data. Cache data is stored locally and is automatically cleared after a certain period of time. Cache does not store any user-specific information.

## Cookie
Cookie is a small text file stored by a website on the client-side (user's browser). It is used to store user-specific information, such as user preferences, login status, and browsing history. Cookies are sent back to the server with each HTTP request, allowing the server to identify the user. Each cookie has an expiration date, after which it is automatically deleted.

## Session
Session is a way to maintain state between a user's interactions with a web application. It is typically used to store user-specific information, such as login status, shopping cart contents, and user preferences. Session data is stored on the server-side, and a session ID is stored in a cookie on the client-side to identify the user. Sessions have a limited lifetime and are usually destroyed when the user logs out or the session expires.

### The difference between Cache and Cookie
- **Cache:** Its function is to make the web page load faster by storing files, images, videos, and other page elements.
- **Cookie:** Its function is to track the user's different browsing activities by storing user-specific information like preferences and login data.

### The difference between Cookie and Session
- **Cookie:** Stored on the client-side, expires after a period of time, and has a maximum size of about 4KB.
- **Session:** Stored on the server-side, ends when a user closes the browser, and typically has no strict size limit.

# How do they work?

This example explains how these three work when shopping online:

- **Cache:** When you revisit an e-commerce website, the site loads faster because data from your previous visit is temporarily stored in the cache.
- **Cookie:** When you try to log in, you may notice your account information is already filled in the login form because cookies have stored that data on your browser.
- **Session:** As you shop, the products you select are kept in the session. This allows the website to remember your choices as you navigate different pages. However, once you close the browser or the session expires, this data is cleared.

# Conclusion

| Feature   | Cache                         | Cookie                                  | Session                               |
|-----------|-------------------------------|-----------------------------------------|---------------------------------------|
| **Location**  | Browser                       | Browser                                 | Server                                |
| **Purpose**   | Speed up page load times      | Preserve user settings, manage login sessions | Maintain user-specific session data |
| **Lifespan**  | Until manually cleared or expired | Set expiration time                | Ends with browser session             |
| **Capacity**  | Large                         | Small (about 4KB)                       | Large                                 |
