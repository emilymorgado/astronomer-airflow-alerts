# Sendgrid Setup

## Create a Sendgrid Token

### [1. Create a New Account](`https://signup.sendgrid.com`)

You will need fill out some boilerplate survey information, but it's a quick setup.

![Sendgrid Signup](https://assets2.astronomer.io/main/docs/emails/sendgrid_signup.png)

A new account will be given 40k emails for the first 30 days and then 100 emails/day for free. As long as your DAGs are working properly, this should be more than enough for most use cases.

After you create your account, you'll reach a view like below.

### 2. Click on "Integrate using our Web API or SMTP relay"
![Sendgrid Getting Started](https://assets2.astronomer.io/main/docs/emails/sendgrid_getting_started.png)

### 3. Choose the "Web API (recommended)" method
![Sendgrid Setup Method](https://assets2.astronomer.io/main/docs/emails/sendgrid_setup_method.png)

### 4. Select the "cURL" option from the languages
![Sendgrid Language](https://assets2.astronomer.io/main/docs/emails/sendgrid_language.png)

### 5. Create a new API Key

The name can be anything you want. Then, run the code in your terminal and verify the integration.

You don't have to export the API Key as the docs suggest. Instead, just replace the `$SENDGRID_API_KEY` in the example code with the generated API key.

**Note:** If you don't execute the code to use the token at least once, Sendgrid will not be able to verify that it is working properly and you will not be able to use the key.

![Sendgrid API Key](https://assets2.astronomer.io/main/docs/emails/sendgrid_apikey.png)

### 6. Verify

On the following page, click `Verify`. This is to make sure that Sendgrid activates the key.

If an error pops up that Sendgrid cannot find the test email, run the cURL command again in your terminal and click `Retry`.
