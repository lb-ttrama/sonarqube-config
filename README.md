Sonarqube scanner for local analysis of LB code.

To run: simply execute `docker-compose up`.

Access `localhost:9000` with the default user/password admin:admin.

Create a new manual project, and create an authentication key to use with it.   

Then, run a baseline evaluation of your main/develop branch.   
```
git checkout develop 
git pull
npm run test:cov
sonar-scanner \
  -Dsonar.projectKey=engagement-api \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.branch.name=develop\
  -Dsonar.login=<login key>
```

After the initial analysis, individual pull requests can be analyzed.  

```
sonar-scanner \
  -Dsonar.projectKey=engagement-api \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.pullrequest.branch=<branch_name> \
  -Dsonar.pullrequest.key=<pull_request_id>\
  -Dsonar.pullrequest.base=develop \
  -Dsonar.login=<login_key>
```
