---
title: "Airflow Alerts"
navTitle: "Airflow Alerts"
description: "How to configure task-level Airflow alerts on Astronomer to monitor task failures, successes and retries."
---

# Airflow Email Alerting

Airflow emails are a useful way to get notified of [DAG](https://github.com/apache/airflow/blob/v1-10-stable/airflow/models/dag.py#L94) successes, retries, failures, and [more](https://github.com/apache/airflow/blob/master/airflow/utils/email.py). Astronomer does not automatically bundle in an SMTP service. So, to send emails through Airflow, you will need to:

1. Set up an SMTP service.
2. Add your SMTP credentials to your Astronomer deployment.
3. Select the alerts you want to run.
4. Implement any additional alerts.

## Setting up an SMTP Service

Below are guides that explain how to set up an SMTP service:
- [Sendgrid](./smtp-setup/sendgrid-setup.md)
- [Amazon's Simple Email Service (SES)](./smtp-setup/amazon-ses-setup.md)

## Adding SMTP Credentials to your Astronomer deployment

1. Once you have an SMTP API key, go to your Astronomer deployment.
2. From the top navigation, click **Configure**.
3. On the left menu, click **Environment Variables**.
4. Your SMTP API key will be used as the value for `SMTP__SMTP_PASSWORD`.
5. Your `SMTP__SMTP_MAIL_FROM` value will be the email you used to sign up with your SMTP service.

Your Environment Variables will look something like:
  <details>
    <summary>Sendgrid Environment Variables
    </summary>
    <p>

    ```shell
    AIRFLOW__SMTP__SMTP_HOST=smtp.sendgrid.net
    AIRFLOW__SMTP__SMTP_STARTTLS=True
    AIRFLOW__SMTP__SMTP_SSL=False
    AIRFLOW__SMTP__SMTP_USER=apikey
    AIRFLOW__SMTP__SMTP_PASSWORD={SENDGRID_APIKEY}
    AIRFLOW__SMTP__SMTP_PORT=587
    AIRFLOW__SMTP__SMTP_MAIL_FROM={FROM_EMAIL}
    ```
    ![Astro Create Envs](https://assets2.astronomer.io/main/docs/emails/astro_create_envs.png)

    </p>
  </details>

  <details>
    <summary>Amazon SES Environment Variables</summary>
    <p>

    ```shell
    AIRFLOW__SMTP__SMTP_HOST="{ASTRONOMER_EMAIL}-smtp.us-east-1.amazonaws.com"
    AIRFLOW__SMTP__SMTP_PORT=587
    AIRFLOW__SMTP__SMTP_STARTTLS=True
    AIRFLOW__SMTP__SMTP_SSL=False
    AIRFLOW__SMTP__SMTP_USER={USERNAME_FROM_STEP2A}
    AIRFLOW__SMTP__SMTP_PASSWORD={PASSWORD_FROM_STEP2A}
    AIRFLOW__SMTP__SMTP_MAIL_FROM={FROM_EMAIL}
    ```

    </p>
  </details>
  <br/>

6. Once everything is set, click **Update** to save your configuration. This will automatically redeploy and propagate your deployment.

## Triggering Alerts on a DAG Run

There are two ways to set up email alerting.

1. Using `email_on_failure`. With this option, an email for each task failure will be sent, even if multiple tasks fail for related reasons.

2. If you're interested in limiting failure alerts to the DAG run level, you can instead pass `on_failure_callback` directly in your DAG file to define a Python function that sends you an email denoting failure. ([source](https://github.com/apache/airflow/blob/v1-10-stable/airflow/models/dag.py#L167))

```
 :param on_failure_callback: A function to be called when a DagRun of this dag fails.
 ```

The code in your DAG will look something like ([this](https://github.com/apache/airflow/blob/v1-10-stable/airflow/utils/email.py#L41)):

```
from airflow.models.email import send_email

def new_email_alert(self, **kwargs):
  title = "TEST MESSAGE: THIS IS A MODIFIED TEST"
  body = ("I've now modified the email alert "
                "to say whatever I want it to say.<br>")
  send_email('my_email@email.com', title, body)
```

## Additional Alerting - Deployment-Level

In the Astronomer UI, you can subscribe to additional alerts in the **Alerts** tab. These alerts are platform level alerts that pertain to how the underlying components are performing. For example, you might choose to receive an alert regarding your scheduler health, or an abnormal rate of task failure.

You do **not** need to create an SMTP URI for this feature to work.

## Airflow Deployment Alerts

| Alert | Description |
| ------------- | ------------- |
| `AirflowDeploymentUnhealthy` | Airflow deployment is unhealthy, not completely available. |
| `AirflowFailureRate` | Airflow tasks are failing at a higher rate than normal. |
| `AirflowSchedulerUnhealthy` | Airflow scheduler is unhealthy, heartbeat has dropped below the acceptable rate. |
| `AirflowPodQuota` | Deployment is near its pod quota, has been using over 95% of it's pod quota for over 10 minutes. |
| `AirflowCPUQuota` | Deployment is near its CPU quota, has been using over 95% of it's CPU quota for over 10 minutes. |
| `AirflowMemoryQuota` | Deployment is near its memory quota, has been using over 95% of it's memory quota for over 10 minutes. |

### Example Alert

This alert fires if the scheduler is not heartbeating every 5 seconds for more than 3 minutes:

```
alert: AirflowSchedulerUnhealthy
      expr: round(rate(airflow_scheduler_heartbeat{type="counter"}[1m]) * 5) == 0
      for: 3m # Scheduler should reboot quick
      labels:
        tier: airflow
        component: scheduler
        deployment: "{{ $labels.deployment }}"
      annotations:
        summary: "{{ $labels.deployment }} scheduler is unhealthy"
        description: "The {{ $labels.deployment }} scheduler's heartbeat has dropped below the acceptable rate."
```

The full PQL ([Prometheus Query Language](https://prometheus.io/docs/prometheus/latest/querying/basics/)) for how all these alerts are triggered can be found in our helm [helm charts ](https://github.com/astronomer/helm.astronomer.io/blob/387bcfcc06885d9253c2e1cfd6a5a08428323c57/charts/prometheus/values.yaml#L99
).

> **Note:** Customizing these alerts is currently only available to Enterprise customers.
