# Angular and Camunda

## Angular
* Angular can be integrated with the Camunda workflow engine using the standard REST API.
* We can reuse Angular's Form Component for loading our UserTask forms based on the FormKey.
* Links with examples:
	* https://camunda.com/blog/2018/02/custom-tasklist-examples/ 
	* https://github.com/abryukhov/angular-bpmn-io 
	* https://github.com/iceman91176/camunda-angular-app 
	* https://github.com/marwenmselmi/camunda-angular-6-8-9
           
* Всички пренаписват този (rewrite of the "main" project):            
	* https://github.com/narve/ang2-bpmnjs 



## Camunda

https://consult-chandra.medium.com/camunda-e08ca8cfda5d
https://docs.camunda.org/manual/7.3/api-references/rest/
        
* User interface
	* Communicate with Getway
	
* Getway
	* Light Java Spring Boot app;
	* Deployed on Tomcat;
	* Fronting to camunda rest engine;
	* Communicate with Parent Process;
	* Trim the response form back-end and take only needable data to the font-end;
		1. Client inits the requests 
		2. Getway Process creates a business key and starts the Parent Process 
		3. Parent Process returns list of commands to Getway (camunda HTTP API) 
		4. Getway Process returns simplified payload and business key 
		5. Client issies CONTINUE with business key and valid actions
	
- Parent Process
	* Orcestring components in the processl;
	* Camunda processes (the logic);
	* Calling subprocesses (business implementation);
	
*  Parent process - orcestration of sub-processes / state managment;
-  Sub-process - business related BPMNs, storage, service calls
