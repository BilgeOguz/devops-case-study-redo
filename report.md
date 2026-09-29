   # Case-Study Report
   *This is a report kept of the*
   *project* **case study**. *Because of my unsatisfactory*
   *work on the previous case, this will be well documented*
   *and explained step by step through.*   
   
   ---
## Case Study Expectations
  ####  Required Section 
  :white_check_mark: Dockerize the application   
  :white_check_mark: Provide a docker compose file to run it locally    
  :white_check_mark: Run minikube/kind or any kind of local kubernetes cluster locally.    
  :white_check_mark: Provide a script and/or the documentation of how to run the cluster locally.   
  :white_check_mark: Prepare kubernetes manifests(yaml files) for the application and for the DB of the app   
  :white_check_mark: Develop a helm chart for the app   
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
   | | - Too many problems w/ helm, going back to courses to refresh memory | 
   | 28.09 | - Created a very simple helm chart in order to move on with the project req |
   | | - Onto creating CI pipeline |
   | | - Refreshing memory by going through courses |
   | | - Created ci jobs for unit testing and docker, but there seems to be issues |
   | | - Troubleshooting the ci | 
   | | - Problem seems to be with the app itself and not the ci, it doesn't pass the tests |
   | | - Troubleshooting the app | 
   | 29.09 | - Problem handling starting from unit to ci |
   | | - moving on to creating a documentation instead | 
   | | - Finishing the ci jobs (they don't actually need to be validating what i have if it's broken. So its better to finish them up and return for why the app is failing tests later | 
   
---
   ## Notes / Problems / Missing Parts 
*This part of the report is for keeping track of pins i've decided*
*to solve later, after initial deadline*
### Unit  
> **1. 404 not found when cURL'ed (most important)**  
> 2. How to **NOT** hardcode the mongodb cred.s in flasks 'db_config.json' [^1] [^8]      
> 3. After i fixed the db config, i ran into an error:
 ```bash
 docker run flask-api:1.1
Traceback (most recent call last):
  File "/src/app.py", line 2, in <module>
    from models import todo  # call model file
    ^^^^^^^^^^^^^^^^^^^^^^^
  File "/src/models/todo.py", line 2, in <module>
    from factory.database import Database
  File "/src/factory/database.py", line 5, in <module>
    from config import config
  File "/src/config.py", line 3, in <module>
    config = json.load(open('db_config.json'))  #load db in json format
  File "/usr/lib/python3.14/json/__init__.py", line 298, in load
    return loads(fp.read(),
        cls=cls, object_hook=object_hook,
        parse_float=parse_float, parse_int=parse_int,
        parse_constant=parse_constant, object_pairs_hook=object_pairs_hook, **kw)
  File "/usr/lib/python3.14/json/__init__.py", line 352, in loads
    return _default_decoder.decode(s)
           ~~~~~~~~~~~~~~~~~~~~~~~^^^
  File "/usr/lib/python3.14/json/decoder.py", line 345, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.14/json/decoder.py", line 361, in raw_decode
    obj, end = self.scan_once(s, idx)
               ~~~~~~~~~~~~~~^^^^^^^^
json.decoder.JSONDecodeError: Expecting ',' delimiter: line 9 column 1 (char 155)
```
>> Seems like a syntax problem at first, but i've checked the 'problem' parts and it seems fine. I'm currently looking it up to fix it, soma people had the same problem (same error with valid syntax) but their solutions don't apply to my problem.
>> I'll give working thorugh the python/flask related issues but they are the least important in this case study. If not
>> working, ***I'd have to call it a day*** and continue with the other (helm especially) parts of the project instead.
>>
>> ***Moving along with the not working but running flask-api:1.0 instead of the corrected but crashing 1.1***
    
### Docker  
> 1. Docker security (?) additions mentined by my supervisor.   
> 2. ~~Double-checking health checks done in compose~~  
>> ~~Just because they seemed healthy, doesn't mean they are, maybe the health checks were implemented poorly.~~
>> Checked them, they are not giving healthy results anyways. There is just no halt system for it.
>>> Its RestartPolicy is set to on-failure, but unhealth isn't failure so it doesn't stop.
>>> **I can't have it changed to healthy right no, then it won't work. I'd have to fix the app side first.**

 ### Kubernetes
> 1. This Kubernetes structure is almost as simplest it gets:
>> Deployments + required networking + helm installed db
>>> It's to be upgraded and added onto once the complete project **once the main requirements are met.**

> 2. ~~Readiness Prope~~ & Pod Affinity for flask deployment
>> Figure out how to ~~get readiness prope~~ and fix affinity (They are in the yaml manifests as a comments)
>>> The liveness prope i currently is a better readiness prope, I should find a new liveness prope instead. 

> 3. Scripts for running locally on dif env
>> For the app deployment,
   ```bash
   minikube image build -t flask-api:1.0 app/mvc-flask-pymongo/
   ```
>>   command used

> ~~4. MongoDB using wrong auth information~~ [^2] 
>> ~~Fix it either creating actual manifest files for mongo or finding a way to change the pull via bash or helm options (later add it in scripts)~~    
>> Fixed by pulling the helm chart instead of installing it

> 5. ~~Loadbalancer stuck in pending state (bc of using it in bare metal instead of cloud providors)~~
>> ~~Meh, maybe a nodeport for now? KodeKloud lesson mentioned it acting as nodepoort in bare metal, how to achieve that?~~ [^3]
>> Changed it to a nodeport at helm level, ready to change back from values anytime.

> 6. Look further into some topics in more depth (mainly tu use at helm at this point in the project)
>> Service accounts (k8s security in general)[^4]
>> Gateway api instead of ingress this time [^5]
   
 ### Helm
> 1. Check out using recommended labels further [^6]
> 2. Chart-hookify your helm [^7]
> 3. Implement hpa and gateway api
> 4. Configure the mongodb values
   
 ### Github Actions
> 1. Unit testing & docker problems
>> Figured out it wasn't because of the tests, but because of the flask app itself.
>> Will have to fix. 


[^1]:[Interpretation of config.json - Kubernetes Docs](https://kubernetes.io/docs/concepts/containers/images/#config-json)
[^2]:[Customizing the Chart Before Installing - Helm Docs](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing)
[^3]:[KodeKloud lesson - timestamp: 2:52](https://learn.kodekloud.com/learn/courses/cka-certification-course-certified-kubernetes-administrator/module/c6d2ac7d-8192-4cff-aa54-e36d888c5bd9/lesson/96eb0c1f-48cc-483e-bc1e-9a5a4c9e75e1)
[^4]:[info on security](https://kubernetes.io/docs/concepts/security/)
[^5]:[Gatewayapi guide](https://gateway-api.sigs.k8s.io/guides/)
[^6]:[Recommended Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/)
[^7]:[KK Course on chart hooks](https://learn.kodekloud.com/learn/courses/helm-for-beginners/module/b90a4aa4-31b5-43d3-a7aa-383d48c50db0/lesson/28973a08-1894-4976-ad1c-1df96e338d4c)
[^8]:[Example Project on KK that uses mongodb - timestamp: 3:31](https://learn.kodekloud.com/learn/courses/github-actions/module/6136c7b5-8fe0-4a84-ae77-0274623512d5/lesson/6d590d33-38aa-4982-a7df-318e8bfb74e8)
