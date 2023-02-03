```
minikube start

eval $(minikube docker-env) 

docker build -t springboot-k8s:1.0 .

kubectl run springboot-k8s --image=springboot-k8s:1.0 --port 8080 --image-pull-policy=Never
pod/springboot-k8s created

kubectl create deployment springboot-k8s --image=springboot-k8s:1.0 --port 8080 deployment.apps/springboot-k8s created

kubectl get deployments                  
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
springboot-k8s   1/1     1            1           3s

kubectl get pods                     
NAME                              READY   STATUS    RESTARTS   AGE
springboot-k8s-6f8ddcbb45-wf7kz   1/1     Running   0          11m

kubectl expose deployment springboot-k8s --type=NodePort
service/springboot-k8s exposed

kubectl autoscale deployment springboot-k8s --cpu-percent=50 --min=1 --max=10
horizontalpodautoscaler.autoscaling/springboot-k8s autoscaled

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
```

```
Deployment is responsible to run a set of pods and service gives network access to these pods
```

<img width="1777" alt="Screenshot 2023-02-04 at 12 56 13 AM" src="https://user-images.githubusercontent.com/43849911/216690221-dec619d5-af9a-48f3-9c1f-babb42942c79.png">

<img width="923" alt="Screenshot 2023-02-04 at 12 57 04 AM" src="https://user-images.githubusercontent.com/43849911/216690391-9cf4abf7-da77-4fe8-a07e-ec0ba5f5a209.png">

<img width="722" alt="Screenshot 2023-02-04 at 1 02 28 AM" src="https://user-images.githubusercontent.com/43849911/216691390-25d668dc-fe75-4caf-b6de-a1a43c516135.png">

<img width="1122" alt="Screenshot 2023-02-04 at 1 55 15 AM" src="https://user-images.githubusercontent.com/43849911/216703733-3e77562c-03c2-4e6a-b357-19e2cf38a223.png">

<img width="718" alt="Screenshot 2023-02-04 at 1 55 32 AM" src="https://user-images.githubusercontent.com/43849911/216703783-d3e18559-0052-4804-860f-c1af676e5cf7.png">


