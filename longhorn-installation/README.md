# Long horn setup document:
### Prerequisites:
###### 1. iscsid
###### 2. NFSv4 client
#####Step_1:- Installing JQ:
```
sudo apt install jq
```
#####Step_2:- Checking Environment/Nodes Script:
```
curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/scripts/environment_check.sh | bash
```
#####Step_3:- Installing open-iscsi:
```
sudo apt-get install open-iscsi
sudo systemctl enable iscsid
sudo systemctl start iscsid
```
#####Step_4:- Installing NFSv4 client and Iscsi:
```
sudo apt-get install nfs-common
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/prerequisite/longhorn-iscsi-installation.yaml
kubectl get pod | grep longhorn-iscsi-installation
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/prerequisite/longhorn-nfs-installation.yaml
kubectl get pod | grep longhorn-nfs-installation
```
#####Step_5:- Installing longhorn:
```
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/longhorn.yaml
kubectl get pods --namespace longhorn-system (to check pods is running or not)
kubectl -n longhorn-system get pod
kubectl get svc -n longhorn-system
```
#####Step_6 Now I am going to use longhorn from outside:
```
kubectl delete service longhorn-frontend -n longhorn-system
sudo vi  longhorn-service.yaml
```
``` 
apiVersion: v1
kind: Service
metadata:
  labels:
    app: longhorn-ui
  name: longhorn-frontend
  namespace: longhorn-system
spec:
  ports:
    - port: 80
      protocol: TCP
      targetPort: 8000
      name: web
  externalIPs:
    - (which we want)
  selector:
    app: longhorn-ui
 ```
 ![image](https://user-images.githubusercontent.com/50055329/188608832-f532f285-7790-4eed-81b5-1a515eda0521.png)
 
- kubectl apply -f longhorn-service.yaml

![image](https://user-images.githubusercontent.com/50055329/188609209-85933d2c-e5cb-401f-9506-abc590985d93.png)
 
 search your ip in google thanks :-)

