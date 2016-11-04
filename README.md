[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/pgraham3/postmark_webhooks/tree/master)

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17418107/95874d6c-5a4b-11e6-9608-6f0e03820bfb.gif)

# Postmark Hooks

This project is a quick start app you can use to host your own URLs for receiving, storing, processing, and viewing webhooks sent from [Postmark](http://postmarkapp.com). It is written in Meteor.js due to the framework's ability to quickly display server side data without needing a refresh in the browser. Be sure to read the [Postmark webhooks documentation](http://developer.postmarkapp.com/developer-webhooks-overview.html). Use the deploy to Heroku button (you can host it somewhere else if you want, like [Galaxy](https://www.meteor.com/hosting)) and begin taking advantage of Postmark's [Open tracking webhooks](http://developer.postmarkapp.com/developer-open-webhook.html), [Bounce webhooks](http://developer.postmarkapp.com/developer-bounce-webhook.html), and [Inbound webhooks](http://developer.postmarkapp.com/developer-inbound-webhook.html).

## Features

- Receive webhook POSTs (well formatted JSON) from Postmark for Bounces, Opens, and Inbound messages, with minimal development/configuration effort on your part
- Automatically send emails (using Postmark) to yourself and/or others when you receive a new bounce, open, or inbound message
- Searchable so you can easily find and view details of a specific bounce, open event, or inbound message

## Prerequisites

- Sign Up for Heroku [here](https://signup.heroku.com)

## Getting Started

- Use the Deploy to Heroku button at the top of this page to quickly host your own instance of the app. 
- Be sure to add a custom name for the app, rather than use one generated by Heroku:

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17383869/2bd1eb66-598d-11e6-81ea-688aa3bedb4a.png)

- Use the custom name to enter in your ROOT_URL for the Heroku app:

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17383895/4c0c025e-598d-11e6-8c39-26b9b020b4eb.png)

- Once you have entered in your custom name and ROOT_URL, you can proceed to deploy the app to Heroku by clicking Deploy for Free. Once completed, click View to open your new app on the web.

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17385099/43fc172c-5995-11e6-8833-7e4a4adb63fc.png)

## Configure Postmark Account

Our next step is to set the new URLs we have available as your settings for your Postmark webhooks. Setting these URLs tells Postmark where to send the webhook messages. URLs for the webhooks are in the following format:

- https://yourname.herokuapp.com/webhooks/bounces
- https://yourname.herokuapp.com/webhooks/opens
- https://yourname.herokuapp.com/webhooks/inbound

If you prefer to use an existing Postmark account you have for sending email notifications, you can remove the Postmark plugin that was added to the app during deployment. Otherwise, access your Postmark in Heroku:

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17417590/f2e34810-5a48-11e6-9d52-e2bad4969b6b.png)

## Set Bounce URL

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17417062/3ef135bc-5a46-11e6-8a2d-bd6767abd99e.gif)

## Set Opens URL

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17417344/b2aa3a48-5a47-11e6-8fd7-7d773c252f06.gif)

Make sure you have Open Tracking enabled to use this webhook. See [this help article](http://support.postmarkapp.com/article/803-how-do-i-enable-open-tracking) for steps on enabling this setting.

## Set Inbound URL

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17416886/29da6406-5a45-11e6-866b-1ab24cfa8c28.gif)

## Included Heroku Add-Ons

Deploying to Heroku sets you up with a Postmark account for sending email notifications from the app that includes 10,000 free emails per month. Additional credits can be purchased as necessary and do not expire. You will also receive an [mLab MongoDB](https://devcenter.heroku.com/articles/mongolab) with their free sandbox plan. You can upgrade your mLab MongoDB plan in Heroku if you need additional storage. Upgrade your mLab MongoDB plan if you plan to use this heavily in production as their sandbox instances do not include redundancy for their databases.

## Built With

* [Atom](https://atom.io)
* [Meteor.js](https://www.meteor.com)
* [Postmark.js](https://www.npmjs.com/package/postmark)

## Security

- Only [Postmark's listed IPs](http://support.postmarkapp.com/article/800-ips-for-firewalls) for webhooks can POST to URLs built by this app. Any other received request is ignored and no document is added to the collections in the database.
- Received data is also validated against a schema to ensure appropriate data received and stored as a document in its associated collection.
- Insecure removed so the database cannot be accessed from the client (browser dev tools)
- Option to receive emails if an unauthorized source attempts to post to your URL

## Modifying Your Application's Codebase

Once you have deployed your own instance of the application to Heroku, you will need to get a local version of the repository where you can make edits and push back to Heroku. If you simply clone your Heroku app's repo, you will end up with an empty repository locally. You can get a local copy of the source code using the following commands:

1. ``heroku git:clone -a YourAppName``
2. ``cd YourAppName``
3. ``git remote add origin https://github.com/pgraham3/postmark_webhooks``
4. ``git pull origin master``

Once you have the local repo for your application, you can then make changes and commit them in the usual fashion. Once you are ready to push your local changes to your Heroku application, use this command:

``git push heroku master``

## Customize Your Notification Settings

This app allows for you to send emails using Postmark when you receive a bounce, open event, or inbound message. Open up server/settings.js to view and modify the settings. Use the Server API Token for the Server you wish to send notifications from in Postmark (found in Credentials when viewing the server in Postmark). You can send emails based on the following events:

* Bounce received
* Inbound Message received
* Open Event received
* Unauthorized (not from Postmark's IP addresses) POST received to one of your webhook URLs

By default, notifications will **not** be sent for events unless you enable them by changing their value to ``true``. Sending notifications for events can be turned on/off for each of these event types by changing these fields to true/false:

* ``SendBouncesNotifications`` (for bounces)
* ``SendOpensNotifications`` (for open events)
* ``SendInboundNotifications`` (for inbound messages)
* ``SendViolationsNotifications`` (for unauthorized POSTs to your URLs)
```javascript
{
  "ServerAPIToken": "YourServerAPIToken",
  "SendBouncesNotifications": false,
  "SendOpensNotifications": false,
  "SendInboundNotifications": false,
  "SendViolationsNotifications": false,
  "BouncesFromEmailAddress": "yourbouncesnotificationemail@yourdomain.com",
  "OpensFromEmailAddress": "youropensnotificationemail@yourdomain.com",
  "InboundFromEmailAddress": "yourinboundnotificationsemail@yourdomain",
  "BouncesToEmailAddress": "pmnotifications+bounces@yourdomain.com",
  "OpensToEmailAddress": "pmnotifications+opens@yourdomain.com",
  "InboundToEmailAddress": "pmnotifications+inbound@yourdomain.com",
  "ViolationsFromEmailAddress": "pmhooksviolations@yourdomain.com",
  "ViolationsToEmailAddress": "pmnotifications+violations@yourdomain.com"
}
```

Set the From email addresses for your notifications to email addresses that you have added as Sender Signatures in Postmark to be sure you are able to send notification emails. These notification emails can be sent to any recipient.

## Note on Inbound Message Size Limits

MongoDB has an inherent limit of ~16.8 MB, which means documents (entries in a MongoDB collection) cannot be larger than this amount. If you receive an inbound webhook notification larger than 16.8 MB due to attachment size, it will not be stored in your collection or display in the Inbound tab and you will see a MongoDB error in your Heroku Logs:

``Error: Document exceeds maximum allowed bson size of 16777216 bytes``

## Authors

* **Patrick Graham**

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Meteor Community
* Mark Otto | Bootstrap
* Tom Coleman | Iron Router & Atmosphere
* Matteo De Micheli | Easy Search
* Charlie DeTar | Meteor.js buildpack for Heroku
* Rico Sta. Cruz | NProgress
