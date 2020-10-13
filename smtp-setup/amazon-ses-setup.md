# Amazon SES Setup

## Verify Email Addresses

Go to: `AWS Console` > `Simple Email Service` > `Email Addresses`

Here, email addresses need to be verified in order to deliver email to them. If you want to configure alerts to be sent to a Slack channel, you can enter an email addresses from Slack to be verified here.

## SMTP Settings

Go to: `AWS Console` > `Simple Email Service` > `SMTP Settings`

Use the `Create My SMTP Credentials` button, which will generate a username and password. This will look similar to an access and secret access key.

**Note:** You won't be able to access these values again, so consider storing them in a password manager. Note the Server Name and Port on this page as well.
