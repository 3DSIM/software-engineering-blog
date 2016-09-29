+++
author = "ryanwalls"
comments = true
date = "2016-09-29T06:16:24-06:00"
draft = false
share = true
slug = "securing-jenkins-with-auth0"
tags = ["setup", "tutorial", "jenkins", "auth0"]
title = "Securing Jenkins with Auth0"

+++

![Jenkins](/images/posts/Securing-Jenkins-with-Auth0/jenkins.png)
![Auth0](/images/posts/Securing-Jenkins-with-Auth0/auth0.png)

We recently (yesterday) switched our Jenkins authentication from the Github authentication plugin to Auth0.  We had a few reasons for this...

* We wanted more fine grained control over permissions
* We didn't want everyone with Github access to have Jenkins access
* We eventually want single sign-on with other 3DSIM applications
* We were already using Auth0 for other things

Bottom line.  We switched.  The process had some minor quirks, so figured I'd write a guide for my future self to follow next time I have to set this up...

## Configure Auth0

Credit goes to [this question](http://stackoverflow.com/questions/33789104/jenkins-integration-with-auth0) on stackoverflow for getting me headed in the right direction.  Here are the steps I took:

* Before starting, make sure you have at least one user configured in Auth0.  
* Create a new client in Auth0 named "Jenkins".  (I chose "regular web app" for the type, but it doesn't really matter.)
![Create client](/images/posts/Securing-Jenkins-with-Auth0/auth0-create-client.png)

* After creating the client, scroll down on the settings tab to the "Allowed Callback URLs section" and add a callback in this form: `<your jenkins url>/securityRealm/finishLogin`
![Add callback url](/images/posts/Securing-Jenkins-with-Auth0/allowed-callback-urls.png)

* Scroll all the way down in settings tab and click on "Show Advanced Settings", then select the "Endpoints" tab
![SAML Metadata URL](/images/posts/Securing-Jenkins-with-Auth0/saml-metadata-url.png)

* Copy the "SAML Metadata URL" and open a new browser window, paste it, and hit enter.  An XML file should be downloaded.  Save it for when we configure Jenkins.
* Scroll back to the top of the client configuration page and select the "Addons" tab.
* Turn on the "SAML2 Web App" addon.
![Addons](/images/posts/Securing-Jenkins-with-Auth0/addons.png)

* In the configuration box for the addon, make sure and set the `recipient` and `audience` fields to your Jenkins callback URL.
![SAML Config](/images/posts/Securing-Jenkins-with-Auth0/saml-config.png)

* After saving (bottom of the page), click on the "Debug" button.  You will be asked to login.  Login using one of your test user accounts.  If the configuration is successful you should see a page that looks like this:
![Success](/images/posts/Securing-Jenkins-with-Auth0/success.png)

## Configure Jenkins

### Jenkins Plugins
* Install the SAML Plugin: https://wiki.jenkins-ci.org/display/JENKINS/SAML+Plugin
* Install the Role Strategy Plugin: https://wiki.jenkins-ci.org/display/JENKINS/Role+Strategy+Plugin

### Configure Jenkins global security
* Go to "Manage Jenkins" -> "Configure Global Security"
![Configure global security](/images/posts/Securing-Jenkins-with-Auth0/configure-global-security.png)
* Check the "Enable security" checkbox
* Select "SAML 2.0" radio button
* Paste XML from the Auth0 metadata URL downloaded previously into the "IdP Metadata" field
* (Optional) Add the field you want to use for the username.  We are using "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
* Under "Authorization", choose "Role-Based Strategy"
* Click "Save"

### Manage and Assign Roles for Jenkins/Auth0 interaction

* Go to "Manage Jenkins" -> "Manage and Assign Roles" -> "Manage Roles"
* Add any roles that makes sense for your use case and assign them permissions.  In our case we added an "admin" and
"authenticated" role.  (Note that roles are different from groups.  In the default setup, anyone who logs in via Auth0 will be assigned to an "authenticated" __group__.  If you want to use more specialized groups in Auth0, you'll need to add the Auth0 Authorization Extension.  See https://auth0.com/docs/extensions/authorization-extension.)
* To match a role to an Auth0 group (remember they are different), navigate to "Manage Jenkins" -> "Manage and Assign Roles" -> "Assign Roles"
![Assign Roles](/images/posts/Securing-Jenkins-with-Auth0/assign-roles.png)
* Here you can associate Auth0 groups/users (left column) with roles (columns 2+) by clicking checkboxes.  
* We added the `authenticated` group to the `authenticated` role we setup previously.  
* To keep non-authenticated users from seeing any of Jenkins, we unchecked all privileges for the `Anonymous` group.
* Save

## Wrapping up
That's it!  Try it out.  

* Open an incognito window and navigate to your jenkins URL.  
* You should be presented with an Auth0 login like this:
![Auth0 login](/images/posts/Securing-Jenkins-with-Auth0/auth0-login.png)
* Once you login, your email address should show up in the top right corner of your Jenkins dashboard.

Congratulations.  You now can use Auth0 to access Jenkins.  Bonus points: setup SSO with your corporate LDAP, AD, Salesforce, or other identity provider.  

Leave a comment below if you have questions or have any suggestions for improving this tutorial.
