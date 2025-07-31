# Automated Bug Reporting & Triaging Workflow

A n8n workflow designed to automate the reporting and initial triaging of software bugs. The primary goal is to streamline the bug reporting process, ensure consistent detail capture, accelerate triaging, and improve communication within development and QA teams.

## Workflow

<img alt="workflow diagram" src="https://github.com/user-attachments/assets/8089fcef-228b-4fea-ba48-f9b7a341ae2a" />

## Workflow Working

The workflow is triggered by an external system (e.g. a test automation framework) when a test fails. It then processes the incoming bug data, makes decisions based on severity, creates tickets in Jira, and sends notifications.

  * Webhook (Trigger)
    This node is the entry point for bug reports. It listens for incoming HTTP POST requests from external systems (e.g. like Playwright, Cypress, or a custom Selenium listener).

  * Data Extraction and Normalization)
    This node processes the raw incoming JSON payload from the Webhook. It extracts, renames, and normalizes the data fields to ensure consistency for subsequent steps in the workflow.

  * Checks for Screenshot (Conditional Logic for Screenshot Availability)
    This If node checks if a `screenshotUrl` was provided in the incoming payload and if it's valid. This prevents the workflow from failing if no screenshot is available or if the URL is empty/null.

    <img alt="Checks for Screenshot image" src="https://github.com/user-attachments/assets/b8ec4d93-4703-4163-895d-2f431cad4504" />

  * Fetch Screenshot (If exist)
    If a screenshotUrl is present and valid, this node attempts to download the screenshot file from the provided URL.

  * Checks Severity of Bug (Conditional Logic for Triaging)
    This If node determines the severity of the reported bug and branches the workflow accordingly for different Jira priority and notification paths.

    <img  alt="image" src="https://github.com/user-attachments/assets/bfc1ee9c-ed6f-44e9-a736-2841ec9e564c" />

  * Critical/Severe Issue (Jira Ticket Creation - High Priority)
    Creates a high-priority bug ticket in Jira for Critical or Blocker severity issues.

    <img  alt="image" src="https://github.com/user-attachments/assets/638dc418-4dee-40d2-a35d-5cdec1ca550c" />

  * Medium/Low Issues (Jira Ticket Creation - Standard Priority)
    Creates a standard priority bug ticket in Jira for High, Medium, or Low severity issues.

    <img width="404" height="773" alt="image" src="https://github.com/user-attachments/assets/072ed240-67b7-4c1b-9225-23c9da2b88ce" />

  * Send Message to Chat (Slack Notification)
    Sends a notification message to a designated Slack channel, alerting the team about the newly reported bug.

    <img width="402" height="819" alt="image" src="https://github.com/user-attachments/assets/dbf7ac16-32db-4599-950b-7de4cf6657c6" />

## Jira
The created tickets are seen on Jira.

<img width="1600" height="772" alt="image" src="https://github.com/user-attachments/assets/9e48519b-2bf2-4b0f-9150-2b36ac598dda" />

# Slack
The messages are also sent on Slack, so that the team members are informed about the reported bugs.

<img width="1398" height="737" alt="image" src="https://github.com/user-attachments/assets/f778c589-5a41-4b3a-97a0-548abcbbda0a" />


## Credentials Used

* n8n Cloud
* Jira Credentials
* Slack Credentials
* External Test Automation Framework (Postman (here) for POST method)


## Operations and Maintenance
  Monitoring: Regularly check the "Executions" tab in n8n for this workflow to monitor its performance and identify any failures.
  
  Error Handling: Ensure the global error handling workflow (if configured) is active to catch any unhandled errors from this workflow.
  
  Credential Updates: If Jira or Slack API tokens expire or change, update the corresponding credentials in n8n.
  
  Webhook URL: The Production Webhook URL should be treated as a stable API endpoint. Avoid changing it unless absolutely necessary, as it would require updating all external systems that send data to it.
  
  Jira Project/Issue Types: If your Jira project or issue types change, update the "Critical/Severe Issue" and "Medium / Low Issues" nodes accordingly.
  
  Slack Channel: If the notification channel changes, update the "Send Message to Chat" node.
  
  Payload Schema: Any changes to the JSON payload sent by the external test automation framework will require updates to the "Edit Fields" node.
  
  Screenshot Accessibility: Ensure the screenshotUrl remains accessible to n8n. If using cloud storage, manage permissions and retention policies for the screenshots.
