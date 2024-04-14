
minikube start --force   --start minikube
-----------------------------------------------------------------------------------------

minikube ip  --showing minikube ip
-----------------------------------------------------------------------------------------

clone git

git clone https://github.com/chetansomkuwar254/kubernetes-b1.git
-----------------------------------------------------------------------------------------

sudo vim namespace-mainifest.yml  --creat namespace-mainifest file nor clone from git 

kubectl apply -f namespace-mainifest.yml --apply namespace-mainifest file

-----------------------------------------------------------------------------------------
kubectl apply -f pod-mainifest.yml  -- apply pod file
apiVersion: v1
kind: Namespace
metadata:
  name: uat

kubectl apply -f pod-mainifest.yml   creat pod with file or cloe from git

--------------------------------------------------
##code of pod-mainifest
apiVersion: v1
kind: Pod
metadata:
    name: eb-ms-pod
    labels:
      env: UAT-env
      app: eb-ms-app
    namespace: uat
spec:
  containers:
    - name: eb-ms-instance
      image: nginx
      ports:
        - name: http
          containerPort: 80
    - name: eb-tomcat-instance
      image: tomcat
      ports:
        - name: java
          containerPort: 8080
-----------------------------------------------------


-------------------------------------------------------------------------------------------------------------
kubectl get pods -o wide -n uat --check pods

o/p

NAME        READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
eb-ms-pod   2/2     Running   0          25m   10.244.0.3   minikube   <none>           <non

-------------------------------------------------------------------------------------------------------------

kubectl expose pod eb-ms-pod --type NodePort --port 80 -n uat --create the service
-----------------------------------------------------------------------------------------
kubectl get svc -o wide -n uat  --get svc name

o/p

NAME        TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
eb-ms-pod   NodePort   10.109.86.158   <none>        80:32765/TCP   8m29s

-----------------------------------------------------------------------------------------

minikube ssh --take ssh access

curl pod ip and port number

ex curl 10.109.86.158:80

exit

access through expernal cliuster or minikube ip

minikube ip

curl minikube_ip:port_no_of_service(svc)

ex curl 192.168.49.2:32765



apiVersion: v1
kind: Service
metadata:
  name: eb-ms-pod-service
  labels:
      env: UAT-env
      app: eb-ms-app
    namespace: uat
spec:
  type: NodePort
  selector:
    app: eb-ms-app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
