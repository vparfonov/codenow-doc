This document is outlining some deeper requirements related to the CodeNow button and onboarding into Codenvy.  
==============================================================================================================
These requirements are being developed after testing the beta, looking at the market place, and having internal brainstorming sessions.

Throughout this requirements document, we reference the concept of a throw-away (temporary) tenant and a named (permanent) tenant.  Throw-away tenants are destroyed after the user leaves our application.  These are also known as unauthenticated tenants, because users can get into our product without any form of authentication occurring.

USE CASE 1a: Register & Login Options on Home Page - Non GOOG Apps Email
This is the behavior that we currently have on the home page.  Today we let people either register a new account, or to login to an existing account.  We would never allow for a throw-away tenant to be created off of our home page.  We will always want to force a user into registration or login.   The anonymous concept is something we should only introduce to users who are using the CodeNow URL concept.  

TODO 1: At some point, once we have a designer, we should add in “Login” oAuth and field forms on the top of our menu bar that are always there.  Today, it’s not obvious that “register” on the main page is the same as “login” for those people who have already created an account with us.

USE CASE 1b: Register & Login Options on Home Page - GOOG Apps Email
If the user registers with an email address that is mapped to a Google Apps organizational email account, we should offer two choices:

Create a new, named, private tenant not associated with GOOG Apps Organization.
Join the organization’s tenant. The admin of the tenant should be sent an email to accept the request. A notification should also be displayed within the tenant to alert the admin of the request.

USE CASE 2: Code Now → Throw Away Tenant - No Previous Visits to Codenvy.com
In this use case, a user is selecting a CodeNow URL that does not require any authentication.  Also, this is a user where we have no previous tracking history, either from cookies we have established or from a previous login.  As far as we can tell, this is a brand new users.  The goal of this use case is to get the user into the editing / building / compiling window as fast as possible, with zero clicks other than the one used to select the Code Now button.

To do this, we would bypass all of the authentication mechanisms.  We would create a temporary tenant, with a random ID on it.  This tenant would have a fixed lifespan, and we would terminate the tenant automatically after a certain number of hours of inactivity or when the user leaves the product by closing their browser tab.  There would be no way for a user to return to this tenant after they close the browser tab.   If the same user clicks on the same CodeNow button that launched them into Codenvy a second time, they would get another another temporary tenant with a different ID.

The CodeNow URL needs to have parameters that would allow the project to be opened with a specific file pre-loaded, and a single line selected.   If these parameters are not present, then the project would be opened with a customized Welcome page (explaining to the user what just happened).

The user would have no restrictions on any of the features.  They can save files, work with GitHub, and do manual connections to PAAS.  They can build and debug.  They can invite other users to this temporary tenant.   The downside for the user is that they cannot return to the tenant in the future.

We would enable users in a temporary tenant to “convert” to a permanent tenant.  When we convert a temporary tenant to a named tenant, there would be two options: 1) Create a brand new account via registering with us, 2) Login to an existing account.   In either case, after the user makes the connection to the permanent tenant, we would then “copy” the project files from the temporary tenant into the named tenant.  After, we would destroy the temporary tenant.

Even though this use case is for a user we have never seen before, we would still give them the option of creating a new named tenant, or logging into an existing one.  It’s possible that a user just cleared their cache, and it appears they are new to us, but actually have a pre-existing Codenvy account.

We would have special UI requirements to support this use case:
1) Built-in visuals in the IDE that tell the user that they are in a temporary tenant, and that if they “register” or “login” with us, their tenant will become permanent along with their existing temporary project copied over.  Also, the gamification spec applies once a user has registered.   So, the compelling reason for converting to a named account is permanence.  The compelling reason users will fill in a deeper profile of their named account will be getting more free public projects added to their capacity.   We could put this as an OUTPUT window in the bottom of the IDE, or as a window that appears just above the editor window but below the menu structure.   On the top would make sense, as we can emphasize to users what is happening.  We can keep this small and unobtrusive, but informative.

2) We would need to support a login & registration step by step flow within the IDE.   This is slightly different than the registration / login flow that exists on the main Web site.

TODO: Have graphic designer create a nice concept to represent the CodeNow button in its various states: unclicked, clicked once, clicked more than once.

USE CASE 3: Code Now → Throw Away Tenant - User Has Codenvy Account & Not Logged In
In this use case, a user is selecting a CodeNow URL that does not require any authentication.  Through cookies, we have detected that the user has a Codenvy Account & is not logged into it currently.  Note: I would not know how we might detect this, but let’s suppose it’s possible.

In this use case, we will take the user into Codenvy and give them a temporary tenant automatically.  They will not need to authenticate to do this.  Once within the editor, we will have a side window that would tell the user that we are aware that they have a Codenvy account, and if they were to Login now, we will copy the project from their temporary tenant into their named tenant.  We will put the oAuth icons and the login form within this window.  The user can make full use of Codenvy within their temporary tenant even though this login window is available within the product.

This behavior is similar to Use Case 2, but the difference is that our side window is telling the user we are aware they have a Codenvy account and can login at any time.

USE CASE 4: Code Now → User Has Codenvy Account & Currently Authenticated
In this use case, we will also take the user into a temporary tenant automatically, but leave the user still logged in.  In this case, we will present a window off to the side that allows the user to “copy” the project from the temporary tenant into their own, but it will not require any authentication - since they are already authenticated.

If the user has reached the maximum number of projects allowed.  Then upon entering into Codenvy we should bring up a dialogue box that indicates they have reached their maximum number of projects.   We would then point them to our gamification screen that indicates ways they can get more projects by further filling out their profile.

USE CASE 5: Code Now → Permanent Tenant w/ Mandatory Authentication
In some cases, our partners may want to use the CodeNow button, but not allow for a temporary tenant. They will want the users to pre-authenticate before going into the IDE.  An example would be Google.  In this case, if a user is already authenticated in their browser with a Google ID, then when they hit the CodeNow button, we would bring up the oAuth ID selector, and ask them to choose an ID to authenticate with.  In this situation, since they authenticated with an oAuth ID, we would have created a permanent tenant for them (or have them rejoin their already created tenant).  At this stage, the gamification spec would still apply.  We would not display the internals of Codenvy differently for an incoming CodeNow request if the user is forced to authenticate.

In this situation, we would be able to remove 1-2 clicks from the current process.  Today when you click CodeNow, we take you through 3 steps:  1) Which authenticator, 2) authenticate, and 3) Allow Access.  In this model, the authenticator would already be selected and if you are already logged into that authenticator, then only step 3) would be needed.

If the CodeNow URL specifies that this authentication is preferred, AND the user is not authenticated against the authenticator, then this use case becomes identical to the Throw Away Tenant scenario.

TODO: Add in optional mandatory authenticator to CodeNow URL (ie, ANY, GITHUB, GOOG)
