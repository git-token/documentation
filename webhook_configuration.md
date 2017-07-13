### Configuring a Webhook

> We’ll send a POST request to the URL below with details of any subscribed events. You can also specify which data format you’d like to receive. More information can be found in our [developer documentation](https://developer.github.com/webhooks/).

Under the **settings** tab in an organization's GitHub dashboard, click **Webhook** on the left navigation section and add a new webhook.

<img src="./GitHubWebHookSetup.png" >

GitToken, by default, sets the webhook endpoint to be `/gittoken`. This endpoint is customizable in the configuration file of the GitToken Docker service. [^GTKWebHook]

Enter the url of the organization's GitHub webhook endpoint in the `payload URL` field of the webhook settings page. This is the endpoint that will receive POST requests when a contribution is made to any of an organizations' repositories.

Additionally, the organization can filter for which events they would like to receive from the web hook service. The organization may wish to listen only for `push` events, or they may wish to receive individual events, or all events. GitToken provides event handling for all event types and recommends listening for all events.

[^GTKWebHook]: e.g., the webhook endpoint for [GitToken](https://github.com/git-token)'s repositories is `https://GitToken.org/gittoken`
