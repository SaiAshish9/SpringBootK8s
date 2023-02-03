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
springboot-k8s                    1/1     Running   0          2m53s
springboot-k8s-6f8ddcbb45-wf7kz   1/1     Running   0          41s

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


```

```
Deployment is responsible to run a set of pods and service gives network access to these pods

```

<img width="1777" alt="Screenshot 2023-02-04 at 12 56 13 AM" src="https://user-images.githubusercontent.com/43849911/216690221-dec619d5-af9a-48f3-9c1f-babb42942c79.png">

<img width="923" alt="Screenshot 2023-02-04 at 12 57 04 AM" src="https://user-images.githubusercontent.com/43849911/216690391-9cf4abf7-da77-4fe8-a07e-ec0ba5f5a209.png">

<img width="722" alt="Screenshot 2023-02-04 at 1 02 28 AM" src="https://user-images.githubusercontent.com/43849911/216691390-25d668dc-fe75-4caf-b6de-a1a43c516135.png">
