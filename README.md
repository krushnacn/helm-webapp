## Using the Chart to deploy your Application to Kubernetes

In order to use the Helm chart to deploy and verify your application in Kubernetes, run the following commands:

1. From the directory containing `Chart.yaml`, run:  

  **Helm **
  
  ```sh
  helm install webapp-prod webapp -f frontend/values.yaml
  ```
  or
  ```sh
  helm upgrade --install  webapp-prod webapp -f frontend/values.yaml
  ```

  This deploys and runs your application in Kubernetes, and prints the following text to the console:  
  
  ```sh
  Congratulations, you have deployed your Application to Kubernetes using Helm!

  To verify your application is running, run the following two commands to set the POD_NAME and CONTAINER_PORT environment variables to the location of your application:

export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=webapp,app.kubernetes.io/instance=webapp-prod" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
  
  echo "Visit http://127.0.0.1:8080 to use your application"
  
  And then open your web browser to http://127.0.0.1:8080
  
  ```
  
2. Copy, paste and run the `export` lines printed to the console
  eg:
  
  ```sh
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=webapp,app.kubernetes.io/instance=webapp-prod" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  ```
  
3. Open a browser to view your application:  
  Open your browser to `http://127.0.0.1:${CONTAINER_PORT}` from the command line using:
  
  ```sh
  open http://127.0.0.1:8080
  ```

Your application should now be visible in your browser as 
```sh
Welcome to python world!
```


## Using the Jenkins to deploy your Application to Kubernetes
Create a Credentials to push docker images to docker hub
```sh
Jenkins > Manage jenkins > Manage credentials > Click on Jenkins > Click on Global credentials (unrestricted) > Add Credentials
```
![image](https://user-images.githubusercontent.com/11458614/189413353-a36d7fae-27cb-4cfd-bec4-98b5d0f912d1.png)

Install Docker pipeline plugin into jenkins
```sh
Jenkins > manage jenkins > manage plugins > Click on available > add "Docker Pipeline" in the search bar > select the plugin > Click on install without restart.
```


Create a pipeline job like below screenshot
![image](https://user-images.githubusercontent.com/11458614/189412579-b1708f34-77d9-45ea-af29-393d67cdef92.png)

```sh
Copy content of jenkins/Jenkinsfile into pipeline section
```
OR

```sh
Jenkinsfile can be mapped to SCM as this is available in GitHub
 ```

Please Build the pipeline. Once sucessfully built then it will look like below mentioned screenshot

![image](https://user-images.githubusercontent.com/11458614/189416821-62117a78-f2e4-4e9c-99ea-5c5953dd345c.png)

