```
minikube start

eval $(minikube docker-env) 

docker build -t springboot-k8s:1.0 .

/*
kubectl run springboot-k8s --image=springboot-k8s:1.0 --port 8080 --image-pull-policy=Never
pod/springboot-k8s created
*/

kubectl create deployment springboot-k8s --image=springboot-k8s:1.0 --port 8080 
deployment.apps/springboot-k8s created

kubectl get deployments                  
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
springboot-k8s   1/1     1            1           3s

kubectl get pods                     
NAME                              READY   STATUS    RESTARTS   AGE
springboot-k8s-6f8ddcbb45-wf7kz   1/1     Running   0          11m

kubectl expose deployment springboot-k8s --type=NodePort
service/springboot-k8s exposed

kubectl get services                 
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.96.0.1       <none>        443/TCP          23h
springboot-k8s   NodePort    10.109.74.102   <none>        8080:30436/TCP   2m3s

minikube ip
192.168.49.2

http://192.168.49.2:30436/greeting

kubectl get nodes -o wide 
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
minikube   Ready    control-plane   23h   v1.26.1   192.168.49.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://20.10.23

kubectl get nodes -o wide            
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
minikube   Ready    control-plane   23h   v1.26.1   192.168.49.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://20.10.23

kubectl logs springboot-k8s-6f8ddcbb45-wf7kz                                 
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.2.6.RELEASE)

2023-02-03 19:50:50.646  INFO 1 --- [           main] c.s.k.s.SpringbootKubernetesApplication  : Starting SpringbootKubernetesApplication v0.0.1-SNAPSHOT on springboot-k8s-6f8ddcbb45-wf7kz with PID 1 (/app.jar started by root in /)
2023-02-03 19:50:50.649  INFO 1 --- [           main] c.s.k.s.SpringbootKubernetesApplication  : No active profile set, falling back to default profiles: default
2023-02-03 19:50:52.378  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2023-02-03 19:50:52.429  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2023-02-03 19:50:52.430  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.33]
2023-02-03 19:50:52.478  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2023-02-03 19:50:52.478  INFO 1 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1714 ms
2023-02-03 19:50:52.765  INFO 1 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2023-02-03 19:50:53.065  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2023-02-03 19:50:53.069  INFO 1 --- [           main] c.s.k.s.SpringbootKubernetesApplication  : Started SpringbootKubernetesApplication in 3.337 seconds (JVM running for 3.89)

kubectl get svc

kubectl pods -o wide               
NAME                              READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
springboot-k8s-6f8ddcbb45-gwbmx   1/1     Running   0          5m45s   10.244.0.68   minikube   <none>           <none>

kubectl scale --replicas=3 deployment/springboot-k8s

kubectl get pods -o wide                              
NAME                              READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
springboot-k8s-6f8ddcbb45-dcx9v   1/1     Running   0          4s      10.244.0.69   minikube   <none>           <none>
springboot-k8s-6f8ddcbb45-gq4p5   1/1     Running   0          4s      10.244.0.70   minikube   <none>           <none>
springboot-k8s-6f8ddcbb45-gwbmx   1/1     Running   0          9m21s   10.244.0.68   minikube   <none>           <none>

minikube service springboot-k8s
|-----------|----------------|-------------|---------------------------|
| NAMESPACE |      NAME      | TARGET PORT |            URL            |
|-----------|----------------|-------------|---------------------------|
| default   | springboot-k8s |        8080 | http://192.168.49.2:30794 |
|-----------|----------------|-------------|---------------------------|
🏃  Starting tunnel for service springboot-k8s.
|-----------|----------------|-------------|------------------------|
| NAMESPACE |      NAME      | TARGET PORT |          URL           |
|-----------|----------------|-------------|------------------------|
| default   | springboot-k8s |             | http://127.0.0.1:50941 |
|-----------|----------------|-------------|------------------------|
🎉  Opening service default/springboot-k8s in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.


kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
springboot-k8s   3/3     3            3           13m

kubectl get pods       
NAME                              READY   STATUS    RESTARTS   AGE
springboot-k8s-6f8ddcbb45-dcx9v   1/1     Running   0          5m21s
springboot-k8s-6f8ddcbb45-gq4p5   1/1     Running   0          5m21s
springboot-k8s-6f8ddcbb45-gwbmx   1/1     Running   0          14m

// to check replicas
kubectl get rs 
NAME                        DESIRED   CURRENT   READY   AGE
springboot-k8s-6f8ddcbb45   3         3         3       16m

minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:51009/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...


// when it reached 50% autoscale container
kubectl autoscale deployment springboot-k8s --cpu-percent=50 --min=1 --max=10
horizontalpodautoscaler.autoscaling/springboot-k8s autoscaled

kubectl get hpa
// hpa => orizontalpodautoscaler

kubectl get hpa
NAME             REFERENCE                   TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
springboot-k8s   Deployment/springboot-k8s   <unknown>/50%   1         10        3          46m

kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
springboot-k8s   3/3     3            3           27m

kubectl get deployments -o wide
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS       IMAGES               SELECTOR
springboot-k8s   3/3     3            3           27m   springboot-k8s   springboot-k8s:1.0   app=springboot-k8s

Process to update image:

docker build -t springboot-k8s:2.0 .

kubectl set image deployment springboot-k8s springboot-k8s=springboot-k8s:2.0

kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
springboot-k8s-6f8ddcbb45-dcx9v   1/1     Running   0          22m
springboot-k8s-6f8ddcbb45-gq4p5   1/1     Running   0          22m
springboot-k8s-6f8ddcbb45-gwbmx   1/1     Running   0          31m

kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
springboot-k8s   3/3     3            3           31m

kubectl get services   
NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.96.0.1      <none>        443/TCP          24h
springboot-k8s   NodePort    10.103.28.69   <none>        8080:30794/TCP   32m

kubectl describe rs

kubectl describe deployments

kubectl apply -f deployment.yml
kubectl apply -f service.yml

```

```
Deployment is responsible to run a set of pods and service gives network access to these pods
```

<img width="1777" alt="Screenshot 2023-02-04 at 12 56 13 AM" src="https://user-images.githubusercontent.com/43849911/216690221-dec619d5-af9a-48f3-9c1f-babb42942c79.png">

<img width="923" alt="Screenshot 2023-02-04 at 12 57 04 AM" src="https://user-images.githubusercontent.com/43849911/216690391-9cf4abf7-da77-4fe8-a07e-ec0ba5f5a209.png">

<img width="722" alt="Screenshot 2023-02-04 at 1 02 28 AM" src="https://user-images.githubusercontent.com/43849911/216691390-25d668dc-fe75-4caf-b6de-a1a43c516135.png">

<img width="1122" alt="Screenshot 2023-02-04 at 1 55 15 AM" src="https://user-images.githubusercontent.com/43849911/216703733-3e77562c-03c2-4e6a-b357-19e2cf38a223.png">

<img width="718" alt="Screenshot 2023-02-04 at 1 55 32 AM" src="https://user-images.githubusercontent.com/43849911/216703783-d3e18559-0052-4804-860f-c1af676e5cf7.png">

<img width="1044" alt="Screenshot 2023-02-04 at 1 59 36 AM" src="https://user-images.githubusercontent.com/43849911/216704659-87174333-d4e2-4b70-8936-36f98298acfb.png">

<img width="1791" alt="Screenshot 2023-02-04 at 2 00 54 AM" src="https://user-images.githubusercontent.com/43849911/216704820-2220afb7-00fa-4d93-a123-a3a615293a2e.png">

<img width="1791" alt="Screenshot 2023-02-04 at 2 01 28 AM" src="https://user-images.githubusercontent.com/43849911/216705055-1b855773-fc34-4588-bf3b-9d4ea4239af3.png">

<img width="1789" alt="Screenshot 2023-02-04 at 2 01 50 AM" src="https://user-images.githubusercontent.com/43849911/216705112-fc287c3c-e8ff-4e27-be3c-e2ef3d9c9d36.png">

<img width="1791" alt="Screenshot 2023-02-04 at 2 02 25 AM" src="https://user-images.githubusercontent.com/43849911/216705193-99147af6-eb58-4be2-91d6-c2e1f4c62172.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 02 56 AM" src="https://user-images.githubusercontent.com/43849911/216705299-e6b54e4a-71c3-4c0d-b676-5b175a3f9bc6.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 03 18 AM" src="https://user-images.githubusercontent.com/43849911/216705365-ca70935d-a2b3-404a-bc51-c592a0333987.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 03 43 AM" src="https://user-images.githubusercontent.com/43849911/216705426-158b902a-e65f-48f0-b37a-83195323eb94.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 04 15 AM" src="https://user-images.githubusercontent.com/43849911/216705508-d5235b2b-d74e-4591-adcd-34f80fc8c1ae.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 04 53 AM" src="https://user-images.githubusercontent.com/43849911/216705608-f5c79fef-7a62-4b26-9310-370c9eefbbc6.png">

<img width="1792" alt="Screenshot 2023-02-04 at 2 05 22 AM" src="https://user-images.githubusercontent.com/43849911/216705697-68592142-a844-4376-b4d0-c48c61a7bd27.png">

<img width="1043" alt="Screenshot 2023-02-04 at 2 13 20 AM" src="https://user-images.githubusercontent.com/43849911/216706986-a81ee5c9-a9ea-4e0e-a097-bd83830438fd.png">

<img width="760" alt="Screenshot 2023-02-04 at 2 15 52 AM" src="https://user-images.githubusercontent.com/43849911/216707472-3adb0fd5-c560-4243-87a6-6af108497aea.png">

<img width="851" alt="Screenshot 2023-02-04 at 2 16 54 AM" src="https://user-images.githubusercontent.com/43849911/216707620-5a02e550-5211-4c44-b267-3dd1d66adb26.png">


