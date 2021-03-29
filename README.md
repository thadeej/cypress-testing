# Cypress Quickstart

Fork this Repository and then clone it to your local machine

## Features
The main features of this quickstart are:

- GitHub Actions workflow config (cypress-report.yaml) to run the tests and then collate a report and host it on your GH-Pages.
- GH pages Reporting page that updates per commit to main branch. Requires a little configuration change in GitHub
- At a local and GH Pages level this leverages Mochawesome for reporting
- Cypress Experimental Studio to allow easy addition of tests by recording user interaction with front end components.

### How to get up and running locally:

After you clone the repository to your local machine you want to run the following:

`npm install`

To open Cypress:

`$(npm bin)/cypress open` or `npx cypress open`

To run cypress locally on command line and produce a collated HTML report:

`cypress run test`

---

## Local Testing

**OPTIONAL:**
For local testing you will need to add your username and password to `cypress.json`:

```{
"defaultCommandTimeout": 6000,
*** ADD THIS BLOCK and UPDATE VALUES***
"env": {
    "username": "add user",
    "password": "add password"
},
***************************************
"reporter": "cypress-multi-reporters",
    "reporterOptions": {
        "configFile": "reporter-config.json"
    },
    "experimentalStudio": true
}
```

## GitHub Actions and GH Pages

### Addition of User Credentials for Testing

*** NEVER ADD SENSITIVE USER NAME AND PASSWORDS TO SOURCE CONTROL***

If your tests require user authentication and your system provides access to sensitive data I would recommend reviewing a different approach possibly using Hashicorp Vault or some other similar technology that enables temp credential injection per testing cycle.

For injection of credentials into the workflow for GH Actions you will need to add your secrets on GitHub -> Settings -> Secrets -> New Repository secret. Here you can add as many usernames and passwords as you like (with unique names) and reference them in your workflow `cypress-report.yaml` like the following example:

      - name: Run Cypress tests
        run: npm run test:chrome --env username=${{ secrets.USERNAME }}, password=${{ secrets.PASSWORD }}
        continue-on-error: true

This injects environment variables into Cypress to be used in your test runs. Just update the same line in the github workflow named `cypress-report.yaml` similar to above (`--env username=${{ secrets.USERNAME }}, password=${{ secrets.PASSWORD }}`).

### GH Pages Reports

When we commit our code to main our GitHub workflow will be initiated and executed. In our cypress-report.yaml workflow our last job is to deploy the repo to a gh-pages branch. 

You will need to go to your repo settings and update the GH-Pages section to point to the gh-pages branch and use the root folder.

<img src="./GH_pages.png" width="500">

Once you have made your first tests and have pushed/merged your changes into the `main` branch GitHub will pick up your workflow and run the jobs to build and run your tests, collate your reports and publish them to your gh-pages.

*NOTE - Your GitHub repo must be public. Again, please do not put any sensitive test data on there.

### Cypress Experimental Studio

This has been turned on by default in the `cypress.json` and allows the user to add to and update tests on the Cypress GUI without having to write any code. This provides some very easy onboarding for team members that may be new to test automation.

You must be using the GUI to use this feature - `$(npm bin)/cypress open` or `npx cypress open`

You will see a magic wand beside your tests in the Cypress Test window, click it to begin the session interaction recording. Save your changes when you are happy and the new tests and/or test commands will be saved to your test spec.

