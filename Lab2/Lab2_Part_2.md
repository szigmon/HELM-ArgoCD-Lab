# HELM-ArgoCD-Lab2-Part-2

## In this lab we will Add More Configurations to our HELM Chart Application

1. In our application we have expose 2 url paths for probes

   - Readiness Path
   - Liveness Path

   In this part we are going to add those probes to our application and test to see how each of them affect the application.

2. Navigate to our deployment.yaml file under helm/templates, and edit the file as following

   - Add Probes to the Deployment

   Add Readiness Probe

   ```YAML
   ...
                 containers:
                 ...
                     readinessProbe:
                       httpGet:
                         path: /health/readiness # this check the application url path
                         port: {{ .Values.containers.containerPort | quote }}}} # In which port the Application is listening
                         scheme: HTTP
                       initialDelaySeconds: 1 # the time is waiting befor testing the application path
                       timeoutSeconds: 1 # the time for timeout
                       periodSeconds: 10 # the abount of time to wait between checks
                       successThreshold: 1 # count to decalre seccessfull 
                       failureThreshold: 3 # count to decalre failure 
                 ...
   ```

   Add Liveness Probe

   ```YAML
   ...           
                   containers:
                   ...
                       livenessProbe:
                         httpGet:
                           path: /health/liveliness
                           port: {{ .Values.containers.containerPort | quote }}
                           scheme: HTTP
                         initialDelaySeconds: 1
                         timeoutSeconds: 1
                         periodSeconds: 10
                         successThreshold: 1
                         failureThreshold: 3
                   ...
   ```

3. For improve transperasy of our application we can set an Enviorment variable with the Port Number of the application.

   - in our application we also preperd it to consume Enviourment variable for the Application port
   - Lets add this variable to the deployment, and occupay it with our Value of port from the HELM Chart

   ```YAML
   ...           
                 containers:
                 ...
                       env:
                         - name: PORT
                           value: {{ .Values.containers.containerPort }}
                 ...
   ...
   ```

4. Lets add some new features to our web site, edit the HTML file as follow.

   - under helm/html, open the index.html file and add

   ```html
   <html>
   <head>
     <title>Helm Chart Application</title>
     <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
     <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css">
     <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap-theme.min.css">
     <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js"></script>
   </head>
   <body>
   </div>
     <div style="margin:100px;">
      
   <class="navbar navbar-inverse navbar-static-top">
     <div class="container">
       <a class="navbar-brand" href="/">Helm Chart Application</a>
       <ul class="nav navbar-nav">
         <li class="active">
           <a href="/">Home</a>
         </li>
         <li>
           <a href="/health/liveliness">Check liveliness</a>
         </li>
         <li>
           <a href="/health/readiness">Check readiness</a>
         </li>
       </ul>
     </div>
   </nav>
       <div class="jumbotron"  style="padding:40px;">
         <img src="https://developers.redhat.com/sites/default/files/styles/article_feature/public/blog/2018/05/openshift-featured.png?   itok=g0Ee8H1H" alt="OpenShift">
         <img src="https://www.nclouds.com/img/services/toolkit/argocd.png">
         <img src="https://raw.githubusercontent.com/rhilconsultants/Application-Deployment-Workshop/main/Class%20artifacts/helm-icon-color.png">
         <h1>Hello, world!</h1>
         <h2>Our New web site, Deployed with HELM and ArgoCD.</h2>
     </div>
   </body>
   </html>
   ```

   Now push all those changes to the Repository

   ```Bash
   git add .
   git commit -m "added probes and updated the web site"
   ```

5. Refrash the argoCD ui and let the application to sync.

   - Now refresh or Open the URL again to see the new web site.
   ![Testing Probes]()