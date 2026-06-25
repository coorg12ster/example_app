
\
Install docker client, docker-engine\
`brew install docker`\
`brew install colima`\
`colima start --edit`

 Increase the CPU to 2 and RAM to 8 sonarQube need higher memory colima starts docker deamon with 2gb by default\
`cd example_app`\
`docker compose up `

Output \
 ✔ Volume example_app_registry_data        
 ✔ Volume example_app_jenkins_home         
 ✔ Volume example_app_postgres_data        
 ✔ Volume example_app_sonarqube_data       
 ✔ Volume example_app_sonarqube_logs       
 ✔ Volume example_app_sonarqube_extensions 
 ✔ Container jenkins                       
 ✔ Container local-registry                
 ✔ Container postgres                      
 ✔ Container sonarqube                     

## SonarQube configuration
In browser http://localhost:9000 \
Login to sonarQube with *admin/admin* it will take you password reset page and update the password
\
\
Create Project > Manually \
Enter Project Key : spring-app\
branch where you repo is there default : name \
Select Local > Then click 'Generate' > copy token > Select maven 

## Jenkin Configuration
Execute in terminal
`docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword` 
Copy the initial password and Open http://localhost:8081/ in Browser follow the step with default option and also reset the password.

In Jenkin click on setting icon on right-top corner > Plugin > Available Plugin \
Install the following plugin
* Docker
* Docker Pipeline
* SonarQube Scanner

Hit Install, in next page Restart Jenkin after installation is complete and wait to restart container to check the status execute the following.
`docker compose ps -a | grep jenkins`
If status show Exited then execute `docker-compose up` again to start the jenkins.

Open Jenkins.
Manage Jenkins → Credentials
Select the appropriate credential store (usually Global).
Click Add Credentials.

Choose:
Kind: Secret text

Enter:

Secret: SonarQube Token ID\
ID: sonar-token\


Click Save.

Same for the Synk 

Secret: Snyk Token ID\
ID: snyk-token\


# copy temporary password 

Open 
http://localhost:8081/

