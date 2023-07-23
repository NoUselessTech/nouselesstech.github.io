---
title:  "Wiz, 0558, Microsoft and Devs"
layout: post
categories: Misc
---

![Coastal Overview](https://images.pexels.com/photos/635356/pexels-photo-635356.jpeg?cs=srgb&dl=pexels-pok-rie-1130268.jpg&fm=jpg&h=500&w=1500&fit=crop)


## It's shit out there

Let's not mince words, the Microsoft leak and the quagmire we find outselves in
as a result is a major concern for organizations and security professionals. One
of the major fears of cloud reliance has been realized and there's so little 
data to help us understand the true impact that it's causing a lot of warranted
concern. The fallout will likely be felt for years in a way that RSA can relate
to from their own loss of keys.

## Wiz to the headlines

When Wiz dropped their article this week, it helped tease out some of the data
that Microsoft had not been as forthcoming about. It was written in partnership
with Microsoft and they (Microsoft) get some credit for that, but it's a bit 
frustrating that it took a third party for more information to hit the internet.
Microsoft should have done better.

As Wiz went through potential risks and software mechansisms, a specific area
caught my attention. It called out Microsoft's `/common` endpoint a misconfig,
but this is a misnomer and needs to be addressed. The common endpont is a 
critical component of the Microsoft OAuth ecosystem as it allows cross tenant
authorization without requiring the up front knowledge of tenant ids or the 
"on.microsoft.com" domain of the incoming user. 

## What is `/common`

Aside from being a legit question about cultural norms, the /common endpoint is
a way for Microsoft to provide easy federation across multiple tenants. In fact,
when you initially logon to many applications with "Login with Microsoft" or 
just go to something like "portal.azure.com", you are originally sent to
Microsoft's commond endpoint to then be redirected to the appropriate IDP for
authentication (OIDC) and authorization (OAuth) of the SP.

This is a VALID configuration. Microsoft uses it. Auth0 fully supports it. It is
the best and easiest way to avoid misconfigurations in your application that 
serves a variety of tenants. The alternate options start to get a bit tricky.

## What if you are `/uncommon`

When the common endpoint is used all authenticatoin requests start with:

`https://login.microsoftonline.com/common/login`

If you do not use the login endpoint, then you must specify the tenant like so:

`https://login.microsoftonline.com/peoplesdynamics.onmicrosoft.com/login`

or

`https://login.microsoftonline.com/0247e98e-8cdc-405d-9ff0-5f9aaf9716d8/login`

At this point, the user authenticating is authenticating exclusively against the
path specified tenant and cannot authenticate to their own tenant for final auth. 
As a result, if the user is not a party of the poited to tenant, their account 
will fail authentication and they will not be able to proceed. This is a 
terrible UX and requires that the application store mappings of users to domains
in advance as doing tenant lookups is technically possible but not the preferred
method of integration.


## Developers are not lazy in this case

The failure here is not in the developers. This is purely a key management issue
and trying to point to developer "misconfigurations" is not only a distraction, 
it is a downright disservice to the great work that has been done. I will again
point to the fact that logon to portal.azure.com uses the common endpoint that 
Wiz inappropriately called a misconfiguration. (Don't believe me, F12 it baby!)

## Curious about your own apps?

I have experience deploying applications in Microsoft's ecosystem, including in
the Azure Application Gallery. I've had the opportunities to walk through code
with representative from Microsoft to ensure that I followed the correct steps
in authX and integration from start to finish. I have been on this block for a
long time.

If you want to ensure your applications are configured well, I'm happy to give
that guidance. Just contact me through any of the methods listed in the nav bar.
It is important to note that I can help in a number of ways including SSO, cloud
native deployments and general best practices.

## Wrap up.

First, thank you Wiz for bringin out clarity to the attack concerns.

Second, Wiz, do better with how you go about calling things misconfigurations.

Third, Microsoft, you should have been leading this conversation from the start.