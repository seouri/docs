---
title: Step-up Authentication
description: Describes using acr_values and acr claims to perform step-up authentication with Auth0
---
# Step-up Authentication

With step-up authentication, applications that allow access to different types of resources can require users to authenticate with a stronger authentication mechanism to access sensitive resources.

For example, Fabrikam's Intranet requires users to authenticate with their username and password to access customer data. However, a request for access to employee data (which may contain sensitive salary information) triggers a stronger authentication mechanism like multifactor authentication.

You can add step-up authentication to your app with Auth0's extensible multifactor authentication support. Your app can verify that the user has logged in using multifactor authentication (MFA) and, if not, require the user to step-up to access certain resources.

![Step-up flow](/media/articles/mfa/step-up-flow.png)

## Step-up Authentication for APIs

When your audience is an API, you can implement step-up authentication with Auth0 using [scopes](/scopes), [Access Tokens](/tokens/access-token) and [rules](/rules).

::: note
An Access Token is a credential you can use to access an API. The actions that you can perform to that API are defined by the scopes your Access Token includes. The rules are JavaScript functions you can use to run custom logic when a user authenticates.
:::

You can use a rule to trigger the step-up authentication mechanism (for example, prompt MFA) whenever the user requests scopes that map to sensitive resources.

This is best explained with an example.

A user signs into Fabrikam's web app. The standard login gives to this user the ability to interact with their API and fetch the users account list. This means that the Access Token that the client receives after the user authentication contains a scope like `read:accounts`.

Now the user wishes to transfer funds from one account to another, which is deemed a high-value transaction. In order to perform this action, the API requires the scope `transfer:funds`.

The Access Token that the user currently has does not include this scope and the client knows it since it knows the set of scopes it requested in the initial authentication call.

The solution is that the client performs another authentication call, but this time it requests the required scope. The browser redirects to Auth0 and a rule is used to challenge the user to authenticate with MFA since a high-value scope was requested.

Once the user successfully authenticates with MFA, a new Access Token which includes the high-value scope is generated and sent. The client will pass the Access Token to the API which will discard it after verification, thereby treating it like a single-use token.

## Step-up Authentication for Web Apps

If it is a web app that verifies the authentication level, and not an API, then you do not have an Access Token. In this case you can check if a user has logged in with MFA by reviewing the contents of their [ID Token](/tokens/id-token). You can then configure your application to deny access to pages with sensitive information if the ID Token indicates that the user did not log in with MFA.

For details see [Step-up Authentication with ID Tokens](/multifactor-authentication/developer/mfa-from-id-token).

## Keep reading

::: next-steps
* [Authentication policy definitions](http://openid.net/specs/openid-provider-authentication-policy-extension-1_0.html#rfc.section.4)
* [Implement Step-up Authentication with ID Tokens](/multifactor-authentication/developer/mfa-from-id-token)
:::
