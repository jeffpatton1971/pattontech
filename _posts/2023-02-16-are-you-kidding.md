---
layout: post
title:  "Are you kidding me?"
date:   2023-02-16 14:24:00 -0600
categories: blog
tags: February 2023 Development
author: Jeff
comments: true
published: true
---
So my last [post]({% post_url 2023-02-10-securing-secrets-in-visual-studio %}) talked about using secrets and azure key vault, and maybe touched a little on the dangers of committing passwords and the like into a repository. Sigh, so in trying to sort out a problem with why the app wasn't pulling in the secrets from the keyvault I hardcoded the connection string to monogo directly in the code, this would have been nearly a week ago. What did I do today? Well here's the [commit](https://github.com/MultiCloudDeployment/DataLayer/commit/22d4e721c1f7eaa7cbd2509db7ac8a05da3e42b1#diff-0c09fc0fca94716476e058d1525324e536d4c8b776d6a718f25601f9bd78dc01R64) you tell me!

Hope you have a wonderful day, I'm changing all the passwords now, don't be me, don't commit passwords in code please!
