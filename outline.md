# Airflow Alerts Revised Outline

## Core Revisions
1. Change initial guide outline to:
  - Set up an SMTP service.
  - Add your SMTP credentials to your Astronomer deployment.
  - Select the alerts you want to run.
  - Implement any additional alerts.

2. Move Sendgrid and Amazon SES setup guides to separate docs. \
  Why? \
    a. The user might have already set this up. \
    b. The user might use a different SMTP service. \
    c. Moving them to different docs helps to shorten this article and stay relevant to the Airflow Alerts topic.
3. Make "Add Credentials" section SMTP agnostic. The environment variables can live in one codeblock. \
  Ideally, there would be a menu at the top of the codeblock, where the user could click on their SMTP service. This selection would change the code snippet content accordingly (like changing the programming language in Twilio or Stripe docs). I couldn't figure out how to do this with vanilla markdown, so I used collapsible toggles instead.

## Other Changes and Considerations
1. Linking "DAG" to something that explains the acronym and why it's relevant. Directed Acyclic Graphs are common data structures, but I didn't know how they related to Airflow Alerts. It's possible there is more context elsewhere that I'm unaware of, but just in case, I linked it.
2. I went with "an SMTP" but maybe "a SMTP" is preferable?
3. I stuck with calling it an "Astronomer deployment", but I wondered if an "Astronomer instance" could also be accurate.
4. For directions like "Click on..." I made the click target word bold. \
Example: From the top navigation, click **Configure**. \
Instead of: From the top navigation, click "Configure".
5. In general, I incorporated more lists. It's my personal preference when I'm reading technical docs.
6. The final link took me to a 404 page, but maybe I don't have permissions for that page.


### Still needs
1. Some personality! I think I made this a little too dry.
2. I made no changes to the Sendgrid and Amazon SES setup docs.
