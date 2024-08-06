
## Installation
Make sure you have latest Python installed and Pip.
1. Make a folder for the app.
2. Copy the content of the folder of the language you want inside your local folder.
3. Download and install requirements.txt with the following command:
```
pip install -r requirements.txt
```
4. You will need to signup to Mailjet  and enter your info and API keys below.

5. Inside the `app.py` directory run the command bellow to make sure everythging is working and see the errors messages if any. You can access on port 5000
```
flask run --host=0.0.0.0
```
6. If everythings run fine, you can deploy in production with [Gunicorn](https://gunicorn.org/).
```
gunicorn app:app -w 1 --threads 4 -b 0.0.0.0:80 
```
Here the command 
`-w 1` : The number of workers, i recommend on 1 because using a json database it does some glitch with multiple workers.
For faster loading time we use 4 threads which remains on one workers to prevent glitches.
`-b 0.0.0.0:80`: The publish port, use port 80 so you can access it on default http.
`app:app`: Define the file for the app.py .

7. Use the basic logins: (demo/demo) or (user2/password2) 

## Getting started
Steps:

- Login as the demo user.
- Create your own account.
- Delete the default accounts:
	- Log in with your own account.
	- Go to /delete_default_profiles.
	- Enter the password for the current logged username.
	- Hit delete and restart the app.

- Verify that user2 and demo have been removed from the list.
- You are now an admin.
- Head to /env to edit the environment variables in the .env file.
    - FEED_SEND: email address you wish to receive the feedbacks form answers 
    - MAILJET...: enter your Mailjet api credidentials to be able to send emails notifications. You MUST create a mailjet account to make the feature work. 
    - SYSTEM_EMAIL: The email you added to mailjet. All notifications will be send with this one.

## Backups
If you want to backup your data, you just need to copy the `users.json` and `ideas.json` .
Then for restoring, replace them in the folder.


## Pages and Functions
This section will explain the features on each templates.
**This will be separated by templates.**
### Basic to know
Each idea have a unique ID so when you edit ideas or mark as bought we use this ID to change the state. For handling users actions we use their username for login. But for displaying we use the full name which we retreive using a function. 


### Login
**Logic**:
- Retrieve the username and password from the template.
- Hash the password using SHA-1 and the `hashlib` module.
- Compare the hashed password and username (NOT case-sensitive) against entries in `users.json`.
- Set the session cookie upon successful authentication.

**Frontend**:
- The `login.html` template is sourced from [Login from 13](https://github.com/LoginRadius/awesome-login-pages) and not custom-coded.


### Dashboard
**Logic**:
- Load the `dashboard.html` template.
- List users in alphabetical order with the current user at the top.
- Display the current user's full name and birthdate in the sidebar.

**Frontend**:
- Top: Four buttons (sidebar toggle, see bought items, help page, add new ideas).
- Center: User list in the order determined by the backend.
- Sidebar (toggleable): Access account parameters, change password and email address, add users, and log out.


### User Gift Ideas
**Logic**:
- If the current user is accessing their own list, redirect to `my_ideas` to prevent them from seeing which gifts others have bought or added for them.

**Frontend**:
- Each idea is in its own container with the following elements:
    - Title
    - Description
    - Added by (user who added the idea)
    - Button to mark as bought/not bought (with the buyer's name)
    - Delete and edit buttons (if the current user added the idea)
    - Link button (if provided)
- If no ideas are available, redirect to the `noidea` page.



### Edit Ideas
**Logic**:
- Verify if the current user is editing their own list or an idea they added.
- Replace the description and link with the new input values.
- Title editing is disabled to prevent complete changes to ideas.

**Frontend**:
- Two fields (pre-populated with current values): description and link.
- Save changes button.


### Add Ideas
**Logic**:
- Basic route (`add_ideas`):
    - Preselect the user in the drop-down list if accessed from a user list.
    - Extract full names from the JSON and pass them to the HTML template, was built around the usernmae, so need to convert to full names.
    - Title is required; description and external link are optional.
- Secondary route (`add2`):
    - No preselection in the drop-down list as the button is on the main dashboard.

**Frontend**:
- User selection drop-down list.
- Title field (required).
- Description and external link fields (optional).
- Add button.


### Add User
**Logic**:
- Extract required fields (username, password, full name, birthday) from the POST request.
- Extract optional fields (email, avatar) using `request.form.get`.
- Hash the plain text password into an SHA-1 string and save it in the JSON.
- Write the new data to the JSON file.

**Frontend**:
- Four mandatory fields: username (for login), password, full name, birthday (not currently used).
- Optional email field (used for email notifications).
- Avatar selection: choose from six avatars, replace files in `static/icons` with appropriately named PNG files (`avatar1.png`, `avatar2.png`, etc.) if you want different ones.

### Help / FAQ  page
**Logic**:
- Retrieve name, email, and feedback from the POST request.
- Construct and send an email using Mailjet with the provided information.
- Display a success message if the email is sent successfully; otherwise, display an error message.
- Redirect to the feedback form page after submission.

**Frontend**:
- Form fields:
    - Name (required)
    - Email (optional)
    - Feedback (required)
- Submit button to send the feedback.
- Two informational sections:
    - FAQ: Clickable questions reveal or hide answers.
    - Tutorials: Clickable titles reveal or hide content.