---
title: NestJS with Google OpenID Connect Authentication Deploy to Plaform.sh
date: 2020-10-30 08:24:00 +08:00
---

There is many different structure even authentication middleware would be a too messy to [audit with](https://github.com/nestjs/docs.nestjs.com/issues/75).  I had try to find a boilderplate of Node.JS or NestJS that I would love to use, on daily basic. Since all the Javascript's ES6 Standard is landed properly, time to choose the basic poison. I am followed the template of [Stephen Doxsee](https://sdoxsee.github.io/blog/2020/02/05/cats-nest-nestjs-mongo-oidc), better than [Auth0's](https://auth0.com/blog/modern-full-stack-development-with-nestjs-react-typescript-and-mongodb-part-1/) right now, at least right now. 

* NestJS vs Adonis
* https://softwareontheroad.com/ideal-nodejs-project-structure
* https://github.com/async-labs/saas

Although I would also choose and documented boilerplate of ExpressJS, but that too much of scope now, NestJS at least defined a structure, thus simplify the way and forced you to only using certain flow that enforce by NestJS's Middleware

1. Git Clone my fork of https://github.com/edwardchanjw/cats-nest
2. Read the .platform.app.yaml for customized deploy step
3. Setup following to your own environment variable in Management Panel of Platform.sh 

```
OAUTH2_CLIENT_PROVIDER_OIDC_ISSUER=https://accounts.google.com
OAUTH2_CLIENT_REGISTRATION_LOGIN_SCOPE=openid profile
SESSION_SECRET=<super+secret+session+key>
OAUTH2_CLIENT_REGISTRATION_LOGIN_CLIENT_ID=<CLIENT_ID>.apps.googleusercontent.com
OAUTH2_CLIENT_REGISTRATION_LOGIN_CLIENT_SECRET=<CLIENT_SECRET>
OAUTH2_CLIENT_REGISTRATION_LOGIN_REDIRECT_URI=<YOUR_APP_URI>
OAUTH2_CLIENT_REGISTRATION_LOGIN_POST_LOGOUT_REDIRECT_URI=<YOUR_APP_URI>
```

The first three environment variable is static, or the secret is defined by you.

The OAUTH2_CLIENT_REGISTRATION_LOGIN_CLIENT_ID and OAUTH2_CLIENT_REGISTRATION_LOGIN_CLIENT_SECRET is provided by Google's OAuth2 Client. You can refer the tutorial on [dev.to](https://dev.to/imichaelowolabi/how-to-implement-login-with-google-in-nest-js-2aoa).

The OAUTH2_CLIENT_REGISTRATION_LOGIN_REDIRECT_URI and OAUTH2_CLIENT_REGISTRATION_LOGIN_POST_LOGOUT_REDIRECT_URI is from Platform.sh, you can automate it on .platform.app.yaml and I leave it as a exercise for you. Probably I can update to Platform.sh community when I have time to spin up a machine.

That is. I will go back to worked on boilerplate for NestJS, ExpressJS with firebase Authentication and OAuth2, hopefully their different and the minor different between them. 
