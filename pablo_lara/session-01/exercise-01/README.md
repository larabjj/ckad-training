

CKAD Bitnami K8s Training user: pablo_lara
---

# Exercise I: Wordpress + MariaDB with Canary

I've used all files include the folder "exercise-01". Also, I've created a new files like *wordpress-secret.yaml* or *mariadb-secret.yaml* in order to pass sensitive information.

## Commands.bash

Before executing this command, I would recommend executing in the command line
~~~
kubectl delete ns exercise-01
~~~
to ensure that *namespace=exercise-01* it's not exist.

## Liveness & Readiness Probe

In the configuration file, you can see that the Deployment *wordpress-deployment.yaml* and *wordpress-canary-deployment.yaml* it has the same probe. An HTTP request to a specific route server, although I'm not sure that it's working fine.
~~~
livenessProbe:
    initialDelaySeconds: 90
    periodSeconds: 30
    httpGet:
      path: /
      port: 80
readinessProbe:
    initialDelaySeconds: 15
    periodSeconds: 5
    httpGet:
      path: /
      port: 80
~~~

In *mariadb-deployment.yaml* I've used:
~~~
livenessProbe:
  exec:
    command:
    - mysqladmin
    - ping
  initialDelaySeconds: 90
  periodSeconds: 30
readinessProbe:
  exec:
    command:
    - mysqladmin
    - ping
  initialDelaySeconds: 5
  periodSeconds: 5
~~~

At last, I've configured *wordpress-canary-deployment.yaml* like *canary version with wordpress 4.9.8*

Once an executed commands.bash, should see the port assigned to *wordpress-svc* type NodePort to open it in your web browser.
~~~
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
mariadb         ClusterIP   10.105.9.227    <none>        3306/TCP       1s
wordpress-svc   NodePort    10.104.36.239   <none>        80:30277/TCP   1s
~~~
~~~
larabjj@ubuntu:~/Desktop/GitHub/pablo_lara/session-01/exercise-01$ kubectl get pods --namespace=exercise-01
NAME                                           READY   STATUS    RESTARTS   AGE
mariadb-deployment-7c484dd4d5-bjl9b            1/1     Running   0          2m
wordpress-canary-deployment-5d7695ccf5-5hqdt   1/1     Running   0          2m
wordpress-deployment-7dbd5b55dc-8wbqq          1/1     Running   0          2m
wordpress-deployment-7dbd5b55dc-flsng          1/1     Running   0          2m
wordpress-deployment-7dbd5b55dc-nnrbg          1/1     Running   0          2m
~~~

##### Thanks so much.
