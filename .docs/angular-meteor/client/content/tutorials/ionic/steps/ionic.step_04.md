{{#template name="ionic.step_04.html"}}
Our app needs Users and authentication.

Meteor’s authentication system has many possible authentication providers like Facebook, Google Account and [more](http://docs.meteor.com/#/full/meteor_loginwithexternalservice)).

WhatsApp uses SMS authentication so we are going to do the same.

We will use the [accounts-phone](https://github.com/okland/accounts-phone) package for that.

Let’s add that package to our Meteor server by running the following command on the Meteor command line:

    $ meteor add okland:accounts-phone

So save time and not use real SMS in development we can configure the package to have admin numbers that will always work without actually sending SMS message and a token that will always work.

To do that, create a `settings.json` file under the `server` folder and enter the following settings inside:

{{> DiffBox tutorialName="ionic-tutorial" step="5.2"}}

This means we can use the numbers `123456789` and `987654321` and they won’t send real SMS message, and then in the confirmation modal, we can always use `1234` and it will work.

And create `sms.js` file under `server` to define the `accounts` package to use those settings:

{{> DiffBox tutorialName="ionic-tutorial" step="5.3"}}

Now when you run the Meteor server, use the following command line:

    $ meteor run --settings settings.json

Now let’s add Accounts-Phone to our client app. in the client command line type:

    $ bower install accounts-phone --save

Don’t forget to update `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.5"}}

Now let’s create our login flow.

We will start from a login screen where the user can type in its phone number. Then we move the user to a confirmation screen where he can type his confirmation number.

From there we transfer the user to his profile page.

So let’s start with the logic screen.

Add the login state to our routes.js file:

{{> DiffBox tutorialName="ionic-tutorial" step="5.6"}}

and the login view:

{{> DiffBox tutorialName="ionic-tutorial" step="5.7"}}

and controller:

{{> DiffBox tutorialName="ionic-tutorial" step="5.8"}}

Don’t forget to update `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.9"}}

In our Login controller we poping up a modal for the user to include his phone number. then sending that number to the `Accounts.requestPhoneVerification` function. After it sends the SMS, we are redirecting the user to the `confirmation` state.

Let’s add the `confirmation` state to our `routes.js` file:

{{> DiffBox tutorialName="ionic-tutorial" step="5.10"}}

The confirmation view:

{{> DiffBox tutorialName="ionic-tutorial" step="5.11"}}

And the controller:

{{> DiffBox tutorialName="ionic-tutorial" step="5.12"}}

Don’t forget to update `index.html`: https://github.com/idanwe/ionic-cli-meteor-whatsapp-tutorial/commit/643e9924f5af3a33e043158b579652681263fe6d

On the conformation controller we pass the phone number and the verification code to the `Accounts.verifyPhone` function and if successful, we redirect the user to his profile page.

Let’s add the `profile` state to our `routes.js` file:

{{> DiffBox tutorialName="ionic-tutorial" step="5.13"}}

The profile view where the user can update his name:

{{> DiffBox tutorialName="ionic-tutorial" step="5.14"}}

And the controller:

{{> DiffBox tutorialName="ionic-tutorial" step="5.15"}}

Don’t forget to update `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.16"}}

The profile controller is calling the `updateName` Meteor method so let’s create that:

{{> DiffBox tutorialName="ionic-tutorial" step="5.17"}}

Now let’s add some styles to login:

{{> DiffBox tutorialName="ionic-tutorial" step="5.18"}}

And profile:

{{> DiffBox tutorialName="ionic-tutorial" step="5.19"}}

and import them to our main `sass` file:
{{> DiffBox tutorialName="ionic-tutorial" step="5.20"}}
{{> DiffBox tutorialName="ionic-tutorial" step="5.23" filename="scss/ionic.app.scss"}}

{{tutorialImage 'ionic' '7.png' 500}}

Now that we have a login process, we can stop unlogged users from going into the app.

`angular-meteor` provides us with the [requireUser](http://angular-meteor.com/api/auth) function that we can use in the `resolve` of ui-router to make sure unlogged user can’t go inside that route:

{{> DiffBox tutorialName="ionic-tutorial" step="5.24"}}

And let’s handle those unlogged users and redirect them to the `login` state.

Create an `auth.js` file and place this code inside:

{{> DiffBox tutorialName="ionic-tutorial" step="5.25"}}

And don’t forget to update `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.26"}}

But of course that client side authentication is not enough and we need to protect our server as well.

Let’s add security checks inside our server methods:

{{> DiffBox tutorialName="ionic-tutorial" step="5.27"}}

Also, now that we know the logged-in user we can distinguish the message he sent and the messages others has sent.

Let’s update the chat-details view to handle that:

{{> DiffBox tutorialName="ionic-tutorial" step="5.30"}}

and add that information to our messages in the “newMessage” method:

{{> DiffBox tutorialName="ionic-tutorial" step="5.28"}}

and the Optimistic UI one as well:

{{> DiffBox tutorialName="ionic-tutorial" step="5.29"}}

One last things for this chapter - the ability to Logout.

Let’s add a settings view inside a new file called `tab-settings.html` inside the `www/templates/` folder:

{{> DiffBox tutorialName="ionic-tutorial" step="5.31"}}

Add a state for it:

{{> DiffBox tutorialName="ionic-tutorial" step="5.32"}}

A controller that simple call the `$meteor.logout` function by `angular-meteor`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.33"}}

Don’t forget to update `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="5.34"}}

{{tutorialImage 'ionic' '8.png' 500}}

{{tutorialImage 'ionic' '9.png' 500}}


You can download a ZIP file with the project at this point [here](https://github.com/idanwe/ionic-cli-meteor-whatsapp-tutorial/archive/20252b19bb04ad71a3974835acacfe06034dfc6f.zip).

{{/template}}
