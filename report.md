   # Case-Study Report
   *This is a report kept of the*
   *project* **case study**. *Because of my unsatisfactory*
   *work on the previous case, this will be well documented*
   *and explained step by step through.*
  
   ---
## Case Study Expectations
  ####  Required Section 
  - [x] Dockerize the application
  - [x] Provide a docker compose file to run it locally 
  - [x] Run minikube/kind or any kind of local kubernetes cluster locally. 
  - [ ] Provide a script and/or the documentation of how to run the cluster locally.
  - [x] Prepare kubernetes manifests(yaml files) for the application and for the DB of the app
  - [ ] Develop a helm chart for the app
  - [ ] Prepare a CI pipeline for the application in any CI tool(Jenkins, Github Actions, GitLab etc.)
     
  ####  Optional / Extra Section
  - [ ] Prepare a CD pipeline as well
  - [ ] Doing every step of the task for a production environment.
  - [ ] Creating a detailed README file.
  - [ ] Preparing automation scripts for any step.(For example to start a Jenkins server, creation of the local kubernetes cluster, deployment of the app to the cluster etc.)


  ---
   ## Logs
   
   | Time  | Actions Taken |
   | ----- | ----------------------- | 
   | 25.09 |  - New git repo initialized| 
   | | -  Decided to use python project. |
   | | - Understood what the project is and how to work with it |
   | | - Started with creating a Dockerfile | 
   | | - Problems with the flask server |
   | 26.09 | - Decided that maybe the problem is that flask is looking for the db to work |
   | | - Created a compose file using docker hub's mongo:7.0.43-jammy |
   | | - Both services work, seem to communicate but still 404 error despite requests going through |
   | | - Tons of research on docker, flask, mongodb |
   | | - ![desktop snapshot](./img-1.png)  | 
   | | - Decided its not a networking issue, putting a pin on it for later |
   | | - Refreshing k8s knowledge by going through some of the courses again |
   | | - Decided to use minikube with this project as well | 
   | 27.09 | - Getting started with raw k8s manifests |
   | | - Removed and reinstalled minikube (broke when i purged docker & old case-study files completely) using minikube docs with stackoverflow for removal |
   | | - Instead of a nodeport, decided to use loadbalancer svc (although it won't work as a load balancer in this k8s env, prev case study had ingress option at helm so load balancing wasn't an issue, might change it later)  |
   | | - Created the simplest working k8s cluster possible for the app. |
   | | - Continuing with helm |
   | | - Installed the db from the bitnami helm chart |
   | | - Creating helm chart for the app |
   
---
   ## Notes / Problems / Missing Parts 
*This part of the report is for keeping track of pins i've decided*
*to solve later, after initial deadline*
> ### App  
>> **1. 404 not found when cURL'ed (most important)**  
>> 2. How to **NOT** hardcode the mongodb cred.s in flasks 'db_config.json' [^1]

> ### Docker  
>> 1. Docker security (?) additions mentined by my supervisor.   
>> 2. Double-checking health checks done in compose   
>>> Just because they seemed healthy, doesn't mean they are, maybe the health checks were implemented poorly.   

> ### Kubernetes
>> 1. This Kubernetes structure is almost as simplest it gets:
>>> Deployments + required networking + pvc for db
>>>> It's to be upgraded and added onto once the complete project requirements are met.

>> 2. Readiness Trope & Pod Affinity for flask deployment
>>> Figure out how to get readiness trope and fix affinity (They are in the yaml manifests as a comments)

>> 3. Scripts for running locally on dif env
>>> For the app deployment,
   '''bash
   minikube image build -t flask-api:1.0 app/mvc-flask-pymongo/ 
   '''
   command used
>> 4. MongoDB using wrong auth information
>>> Fix it either creating actual manifest files for mongo or finding a way to change the pull via bash or helm options (later add it in scripts)   
   
> ### Helm
>> 1. Used the bitnami mongodb chart to create a deployed running instance of mongodb in kubernetes (couldn't get the chart itself, just the release)

---

[^1]:[Interpretation of config.json - Kubernetes docs](https://kubernetes.io/docs/concepts/containers/images/#config-json)


