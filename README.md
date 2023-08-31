# firebase-sample

This is a firebauth authentication sample repo. I want to show that SAML login is not behaving as
I think it should. When a email/password user exists in the system, and SAML is setup, and you try
to log in with the same SAML user, firebase is automatically merging them without any verification.

I describe the steps I took below:

First, create a firebase project. I've deployed the code in this repo with `./node_modules/.bin/firebase deploy`.

Next, I created an okta auth0 account at https://developer.okta.com/signup/. Its the Customer Identity Cloud one.
I don't think idp matters, this one just allows me to create users easily.

In okta, I created an app. Under addons, I enable SAML:
![image](https://github.com/jaym/firebase-sample/assets/27443/3d42d76a-2e33-4ed7-8cc4-58ad1264d0e0)

![image](https://github.com/jaym/firebase-sample/assets/27443/bae85761-cd6c-49b6-9980-d0323830b45e)

The settings were:
```
{
      "audience": "urn:mondoo:saml-sharp-vaughan-129621",
      "nameIdentifierFormat": "urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress",
      "nameIdentifierProbes": ["http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"]
}
```

Grab the `Issuer`, `Identtiy Provider Certificate`, and `Identity Provider Login URL` from the Usage page:
![image](https://github.com/jaym/firebase-sample/assets/27443/a340cbfc-ae39-4cb3-806e-ae4d2bfb025b)


Back in firebase, create an email provider and SAML provider:
![image](https://github.com/jaym/firebase-sample/assets/27443/d1ff0737-9271-4075-9ee3-0aeb0f074299)
![image](https://github.com/jaym/firebase-sample/assets/27443/50a6c856-e495-4f69-8981-7ae5cd9c064c)
![image](https://github.com/jaym/firebase-sample/assets/27443/c2e1c566-3c37-418c-9a34-7767772976d7)

Create a user in the firebase console. Reset its password. You'll receive an email to set the password.

Create a user with the same email in Okta/Auth0.

![image](https://github.com/jaym/firebase-sample/assets/27443/07f2c420-9d14-4ba3-a7e1-9d2f481305c4)

In a private tab or with another browser,
navigate to the deployed app in firebase. For me, that was https://test-base-project-jaym.web.app/index.html.
Click the `Sign In with SAML` button. Login with the Okta/Auth0 idp.

Go back to the firebase console. Under users, the user is now listed with 2 providers. No authentication was
requested.

![image](https://github.com/jaym/firebase-sample/assets/27443/858b1f47-5878-4065-b8b4-9b36ec071105)
