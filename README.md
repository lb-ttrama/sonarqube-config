Community edition of SonarQube with a plugin that enables multi-branch analysis

# Get Started
## Required Dependencies
- [sonar-scanner](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/) - Download zip file and add the folder to your $PATH
- docker - the sonarqube ui and persisted database run locally and is managed using docker
- [github command line](https://cli.github.com/) - simplifies data collection and pull request analysis
- [jq](https://stedolan.github.io/jq/download/) - used to parse data from the gh command line

All of these commands should be executable from any location locally.

## Configuration
*Initial Boot*  
To start, we need to configure the sonarqube UI.   
After cloning the repository, run `docker-compose up` in the root directory to instantiate sonarqube and its database.   
In your browser, navigate to localhost:9000.   
Log in using admin:admin, and when prompted - change the password.  

In the upper right hand corner,  click the square with an 'A' in it to load the admin account information.   
Switch to the security section and generate a new token.   
Copy the value outputted and add it to your global environment variables. `~/.bashrc`, `~/.zshrc`, or [windows directions](https://phoenixnap.com/kb/windows-set-environment-variable) in the variable `SQ_LOGIN_TOKEN`.   EG:  `export SQ_LOGIN_TOKEN=12345`

*Executable scripts*
To make life easier - a set of utility scripts have been created and included in the repo - in the `./scripts` directory.
Make sure this directory is added to your $PATH so the scripts can be executed from anywhere.

## Analyze

*First analysis*
We're going to work backwards and analyze a pull request before the base branch.   This is done to simplify the process of adding a new project and setting the base branch.  

Navigate to one of your repositories and run `scan-pr <ID>`, where ID is one of the github pull request ids for that repo.  This can easily be shown by running `gh pr list`.
If everything is configured correctly, the script will 
1. Switch to the pull request branch
2. Perform a code coverage test
3. Analyze the repository

After this is complete, navigate in your browser to the sonarqube ui and you'll see a new project has been added.  

*Set and analyze the base branch*
Open the newly created project and go to "Project Settings" => "Branches and Pull Requests".  Find the record marked "main branch" and rename the branch to the common trunk (generally develop).   This change adjusts sonarqube so that all pull requests are compared against this branch instead of master.

Perform the initial base branch analysis by running `scan-base`.  Once this is done, you'll be able to see a comparison of a pull request against the base branch each time you run it.

# Everything else
You should be fully set up and able to analyze pull requests as needed. Be sure to analyze the base branch on ocassion to ensure it's up to date.
Your docker containers can be turned down whenever you like because the data is persisted across runs.
