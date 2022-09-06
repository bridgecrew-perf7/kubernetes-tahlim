# Long horn setup document:
- sudo apt install jq
##### Using the Environment/Nodes Check Script:
- curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/scripts/environment_check.sh | bash
##### Installing open-iscsi:
-	sudo apt-get install open-iscsi
- sudo systemctl enable iscsid
- sudo systemctl start iscsid
##### Installing NFSv4 client:
- sudo apt-get install nfs-common
-	kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/prerequisite/longhorn-iscsi-installation.yaml
-	kubectl get pod | grep longhorn-iscsi-installation
-	kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/prerequisite/longhorn-nfs-installation.yaml
-	kubectl get pod | grep longhorn-nfs-installation
-	kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/longhorn.yaml
-	kubectl get pods --namespace longhorn-system (to check pods is running or not)
-	kubectl -n longhorn-system get pod
-	kubectl get svc -n longhorn-system
### Now I am going to use longhorn from outside:
- kubectl delete service longhorn-frontend -n longhorn-system
- sudo vi  longhorn-service.yaml
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

