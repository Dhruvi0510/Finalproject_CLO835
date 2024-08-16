Step 1:

Minikube Start 

docker build -t pateldhruvi7195/nginx -f "C:\Users\patel\Downloads\Projectclo835\nginx-static\Dockerfile.txt" .

docker run -d -p 8080:80 --name nginx pateldhruvi7195/nginx

Step 2: Backend and Database

Create a Node.js API server that connects to MongoDB.
    - mkdir mongo-node-api
    - cd mongo-node-api
    - npm init -y
    - npm install express mongoose dotenv
    
    Build and push the Docker image to Docker HuB
    docker build -t pateldhruvi7195/mongo-node-api -f "C:\Users\patel\Downloads\Projectclo835\mongo-node-api\Dockerfile.txt" .
    
    docker push pateldhruvi7195/mongo-node-api

Step3: Database Setup (MongoDB)

Create persistent volumes
kubectl apply -f persistent-volume.yml
kubectl apply -f persistent-volume-claim.yml

Creating mongo username and password using base64 as that's the input requirement

powershell -Command "[Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes('pateld'))"

powershell -Command "[Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes('your-password'))"

Setting up configuration

kubectl apply -f configmap.yml
kubectl apply -f secret.yml

Setting Up MONGO

 kubectl apply -f mongo-deployment.yml 
kubectl apply -f mongo-service.yml 

Step 4: Deploy Node.js Backend:

kubectl apply -f node-api-deployment.yml
kubectl apply -f node-api-service.yml

Step 5: Nginx deployment

kubectl apply -f nginx-deployment.yml
kubectl apply -f nginx-service.yml
Exposing nginx in the localhost:
  minikube service nginx --url

Step 6: Verifying deployment

kubectl get deployments
kubectl get services
minikube logs
kubectl get pods -o wide
kubectl logs
kubectl describe pod
kubectl delete pod

Step 7: Application Management

For sclaing 
kubectl scale deployment node-api --replicas=2
kubectl scale deployment nginx --replicas=2

status of deployed resources
kubectl rollout status deployment node-api
kubectl rollout status deployment nginx

For Monitoring
kubectl logs
kubectl describe pod
kubectl get pods -o wide

Port Forwarding:
kubectl port-forward mongo-76d8bb58fc-2bglh 27017:27017

Interacting with MongoDB
Install mongo shell
mongosh --host localhost --port 27017

Environment variable verification 
kubectl exec -it mongo-76d8bb58fc-2bglh  -- env | findstr MONGO

kubectl exec -it node-api-79dd956b95-6z576  -- env | findstr MONGO



  


