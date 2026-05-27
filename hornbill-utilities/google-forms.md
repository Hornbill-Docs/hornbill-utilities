# Google Forms
The [Google Forms](https://www.google.ca/forms/about/) integration allows you to interact with the Service Manager app by allowing Google Forms to create new requests or update existing requests using the Google Forms questions and storing the answers back into a request's custom fields.

## At a Glance

### Update an Existing Request
The following steps will be used to allow for a Google Form link to be sent via email from an existing request using either the BPM Workflow or an Auto Task.

1. Set up an API Key.
2. Create your Google Form.
3. Adding the integration script.
4. Customizing the integration script.
5. Add a trigger.
6. Create a pre-filled link.
7. Add the pre-filled link to an automated email.

## Setup an API Key
The creation of an API Key allows for secure authentication for the Google Forms to interact with Hornbill. The API Key will be used within the Integration Script

1. Open the Platform configuration.
2. Navigate to System > Organizational Data > Users.
3. Select a user that you wish Google Forms to authenticate as. This user will require the rights needed to create and modify requests in Service Manager.
4. On the User Account, select the API Key tab.
5. Create a new API Key using the name Google Forms.
6. Keep this page available to you, as you will need to copy the API Key into the integration script.

## Create Your Google Form

This part of the setup requires that you have a Google account.

1. Open your Internet Browser and navigate to google.com and login with the account you wish to use to create the form.
2. From the Google Apps menu, find the Forms app and select to open.
3. Under the _Start a New Form_ section, select _Blank_.
4. Add a title to your form.
5. Click on the default question that has been provided.
6. Change _Untitled Question_ to _Request Reference_. This first question will be used to hold the Request ID that it is linked with and will be prepopulated (Create a Pre-filled Link).
7. Change the question type to _Short Answer_.
8. At this point, you can optionally add more questions, but it is not required. Additional questions will be covered in Map Questions to Custom Fields.
9. Keep this browser tab open as you need it in the next step.

## Adding the Integration Script
This section will take you through taking the provided [example script](https://github.com/hornbill/gsGoogleFormsToHornbill/blob/main/Code.gs) and adding it to your Google Form.

1. View the Example Script that is provided on GitHub (Use CTRL key to open in a new tab).
2. Copy the entire contents of the script by clicking on the _Copy Raw Contents_ button.
3. View the browser tab where your Google Form is located.
4. Click on the More Menu (vertical ellipse button next to your profile picture) and select _Script_.
5. Click on _Untitled Project_ and give your project a name.
6. Delete all of the default text in the code.gs window so you have a blank space.
7. Paste the text that was copied from the Example Script.
8. Save the script.

## Customizing the Integration Script

### Authentication

At the top of the script there are 2 key lines that need editing.

![Google Forms Authentication](/_books/hornbill-utilities/images/google-forms-authentication.png)

* **var apiKey = 'yourapikey';**<br>
    The text _yourapikey_ needs to be replaced with the key that we generated in the section: Setup an API Key. Be sure to keep the quotation marks.

* **var instanceId = 'yourinstanceid';**<br>
    The text 'yourinstanceid' needs to be replaced with your instance ID. This can be found within the URL that connects you to your Hornbill. This should be all lowercase. Be sure to keep the quotation marks.

  
### Request Reference
In the section Create Your Google Form you will have added your first question to your form. This question, titled _Request Reference_ is the link between this form and an existing request in Service Manager. If you wish to have a different title than _Request Reference_ you will need to update the following line of code to match the title of the question.

![Request Reference](/_books/hornbill-utilities/images/google-forms-request-reference.png) 

* **itemResponses\[i\].getItem().getTitle() === "Request Reference"**<br>
    Make sure that the text _Request Reference_ matches the title of the question in the form that will hold the reference ID.

  
### Questions and Custom Fields
Each question that you add to a Google Form can be mapped to a request's custom field. The script uses a case statement to identify the Form question using its title and then maps it to a custom field where the response will be stored.

![Custom Fields](/_books/hornbill-utilities/images/google-forms-custom-fields.png) 

In this example, there are three questions

* **Address**. A question that has the title _**Address**_ will be added to the custom field _**h\_custom\_d**_
* **Contact**. A question that has the title _**Contact**_ will be added to the custom field _**h\_custom\_e**_
* **Date Of Birth**. A question that has the title _**Date Of Birth**_ will be added to the custom field _**h\_custom\_f**_

You can add, remove, rename, each case statement and choose your custom fields to match your Form questions and where you would like the responses to be stored.  
  
### Update Status

![Update Status](/_books/hornbill-utilities/images/google-forms-update-status.png) 

The _Update Status_ function provides two updates that can be made to a request.

* **Status**. The default in this script is set to _Open_. Options can include New, Open, Resolved, and Closed.
* **Update Timeline**. A Timeline entry will be added to a request once the Form has been submitted. Here you can change the text that will be displayed in the Timeline of the request

## Add a Trigger
Triggers are created to specify what happens when someone finishes and submits a form. The trigger will link to the functions within the newly created script.

1. On the left vertical menu bar, click on the clock, which will open the Triggers page.
2. Click on + Add Trigger (bottom right).
3. Under the option _Choose which function to run, make sure that *OnSubmit* is selected.
4. Under *Select Event Type*, make sure that _On form submit_ is selected.
5. Click _Save_.
6. You may be presented with a dialog box asking you to select your Google account.
7. You may be presented with a warning message saying that Google hasn’t verified this app. Click *Advanced*, and then at the bottom select _Go to project (unsafe)_.
8. You may be presented with a dialog box saying your project wants to access your Google Account. Scroll to the bottom and select _Allow_.
9. You may receive a Security Alert email in your Google mailbox.
10. Click _Save_ again.

## Create a Pre-filled Link

Google Forms are generally accessed by sending a link to the person that you want to complete the form. A _Pre-filled Link_ not only provides the link, but it also contains answers to one or more questions that automatically get populated on the form. This will be used to populate the _Request Reference_ question in order to link the Form with an existing Service Manager Request.

1. On the Form Designer page, click on the _More_ menu (vertical ellipse button) located in the top right.
2. Select the menu item labeled *Get Pre-filled Link*.
3. Add a random Request Reference to the Request Reference question.
4. Click on the *Get Link* button.
5. On the bottom left of your screen, you will be presented with an option to **COPY LINK**.
6. Click on **COPY LINK** to copy the link to your clipboard.
7. Test the link by opening a new browser tab and pasting the link in the URL. This will take you to the form with the first answer pre-filled with the request reference.
8. You will notice that within the link, the request reference is clearly visible. Save and store this link for later use within your BPMs or Auto Tasks.

## Add the Pre-filled Link to an Automated Email
Once you have your Pre-filled Link, you are ready to send it to your users. This is typically done using Hornbill Automation in either a workflow or an Auto Task using an email template.

1. Locate the Request Reference within your Pre-filled Link.
2. Create or edit an email template that you wish to use to distribute the Google Form to your users.
3. Add the Pre-filled Link to the body of the template or as a link on some existing text.
4. Replace the Request Reference with the Request ID variable. For example, change ...usp=pp\_url&entry.59323096=IN00000001 to ...usp=pp\_url&entry.59323096={{.H\_pk\_reference}}.