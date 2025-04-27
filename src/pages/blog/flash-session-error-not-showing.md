---
layout: ../../layouts/BlogPostLayout.astro
title: "Why flash session error message not showing?"
slug: flash-session-error-not-showing
publishDate: 2023-04-25
excerpt: "It happened to me many times. I end my method with redirect that has a message that should be flashed to the user but it does not show. Solution is simple, just make sure that there is no redirect is happening."
author: "Ali Awwad"
---

# Why flash session error message not showing?

It happened to me many times. I end my method with redirect that has a message that should be flashed to the user but it does not show.

## The Problem

Solution is simple, just make sure that there is no redirect is happening.

As an example, you may redirect the logged in user after special check, for me it was verfying that the event they are participating in hasn't expired.

So I wrote as below:

```php
return redirect('/')->with('error','The Event has ended.');
```

However, I had a check that when a user visits the root / page, check if they are logged in, if they are not, then lead them to the login route. And this was the issue.

This meant that there is a double redirect happeing:

- Failed login
- Go Root `/`
- hidden Login check
- Redirected to `login` again

This made the session to be lost

## The Solution

Solution is very simple, just make sure you point the redirect to the correct route. In my case it was:

```php
return redirect('/login')->with('error','The Event has ended');
```

This ensures the flash message shows correctly without being lost in a redirect chain.
