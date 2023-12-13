## check systems running

> largely extracted from [here](https://www.baeldung.com/ops/jenkins-war-update)



gets a list of the running services



## jenkins
* list services
	* `systemctl list-units --type=service --state=running`
* get latest war file:
	* `wget https://updates.jenkins-ci.org/latest/jenkins.war`

CI's jenkins
* executable path : 
	* `/usr/share/java/jenkins.war`
	* get this from the manage/system info page `http://192.168.20.23:8080/manage/systemInfo`
* stop the service:
	* `sudo systemctl stop jenkins`
* move the war (after setting it executable):
	* `sudo chown root:root jenkins.war`
	* `sudo chmod 644 jenkins.war`
	* `mv jenkins.war /usr/share/java/`
* start the service:
	* `systemctl start jenkins`



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE1NTU5ODMxMSwtMTQ4MDgxNzU3LDI1Mz
A2NTc0N119
-->