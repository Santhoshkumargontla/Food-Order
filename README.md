# Food Ordering Application

A full-stack Food Ordering Application built using **React.js, Flask, and MongoDB**. The application allows users to browse food items and place orders through a web interface. It uses Docker for containerization and Kubernetes with Minikube for deployment and orchestration.

## Project Overview

The Food Ordering Application demonstrates how a full-stack application can be developed, containerized, and deployed using modern DevOps tools.

The application consists of three main components:

- **Frontend:** React application served using Nginx.
- **Backend:** Flask REST API developed using Python.
- **Database:** MongoDB for storing food order information.

Docker is used to build and run containers, while Kubernetes manages application deployments, services, and persistent storage.

## Features

- Browse available food items.
- View food item names and prices.
- Place food orders through the frontend.
- Send order information to the Flask backend.
- Store order information in MongoDB.
- Retrieve and manage order data through the backend.
- Containerized frontend, backend, and database.
- Kubernetes-based deployment using Minikube.
- Persistent MongoDB storage using Kubernetes PersistentVolumeClaims.

## Technology Stack

| Category | Technology |
|---|---|
| Frontend | React.js |
| Frontend Server | Nginx |
| Backend | Python, Flask |
| Database | MongoDB |
| Containerization | Docker |
| Container Management | Docker Compose |
| Orchestration | Kubernetes |
| Local Kubernetes | Minikube |
| Image Registry | Docker Hub |

## Application Architecture

```text
                  USER
                    |
                    v
          React Frontend (Nginx)
                    |
                    v
             Flask Backend
                    |
                    v
                 MongoDB
                    |
                    v
             Orders Collection
```

### Component Details

**1. Frontend**
- Developed using React.
- Provides the user interface for browsing food items and placing orders.
- Served using Nginx inside a Docker container.
- Communicates with the Flask backend.

**2. Backend**
- Developed using Python and Flask.
- Handles incoming API requests from the frontend.
- Processes food order information.
- Connects to MongoDB to store order records.

**3. Database**
- Uses MongoDB to store order information.
- Runs in a separate container.
- Uses persistent storage in the Kubernetes deployment to retain data across MongoDB pod restarts.

## Project Structure

```text
Food-Order/
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   └── package.json
│
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── docker-compose.yml
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
└── README.md
```

*Note: The structure above illustrates the main project components. Keep the actual filenames and directories from your repository if they differ.*

## Docker Hub Images

The Docker images are published to Docker Hub.

| Component | Docker Image |
|---|---|
| Frontend | `gontlasanthoshkumar2005/food-frontend:v1` |
| Backend | `gontlasanthoshkumar2005/food-backend:v1` |

### Docker Hub Repositories

- [Food Ordering Frontend](https://hub.docker.com/r/gontlasanthoshkumar2005/food-frontend)
- [Food Ordering Backend](https://hub.docker.com/r/gontlasanthoshkumar2005/food-backend)

### Pull Docker Images

```bash
docker pull gontlasanthoshkumar2005/food-frontend:v1

docker pull gontlasanthoshkumar2005/food-backend:v1
```

## Run the Application Locally

### Prerequisites

Install the following tools:

- Node.js and npm
- Python
- Docker Desktop
- Docker Compose

### 1. Clone the Repository

```bash
git clone https://github.com/Santhoshkumargontla/Food-Order.git

cd Food-Order
```

### 2. Run Using Docker Compose

Build and start all application services:

```bash
docker compose up --build
```

To run the containers in the background:

```bash
docker compose up --build -d
```

### 3. Check Running Containers

```bash
docker compose ps
```

### 4. Access the Application

Open the frontend in your browser:

```text
http://localhost:8080
```

The Flask backend runs on port `5000`.

### 5. Stop the Application

```bash
docker compose down
```

To remove the containers and associated Compose network, use:

```bash
docker compose down
```

Do not use `-v` unless you intentionally want to remove the associated persistent volumes and their data.

## Docker Commands

### Build Docker Images

Build the frontend image:

```bash
docker build -t gontlasanthoshkumar2005/food-frontend:v1 ./frontend
```

Build the backend image:

```bash
docker build -t gontlasanthoshkumar2005/food-backend:v1 ./backend
```

### Push Images to Docker Hub

Login:

```bash
docker login
```

Push the frontend:

```bash
docker push gontlasanthoshkumar2005/food-frontend:v1
```

Push the backend:

```bash
docker push gontlasanthoshkumar2005/food-backend:v1
```

## Kubernetes Deployment Using Minikube

The application is deployed to a local Kubernetes cluster using Minikube.

### Prerequisites

- Docker Desktop
- kubectl
- Minikube

### 1. Start Minikube

```bash
minikube start --driver=docker
```

Check cluster status:

```bash
minikube status
```

Check Kubernetes nodes:

```bash
kubectl get nodes
```

### 2. Deploy Application Resources

Apply the Kubernetes deployment and service manifests from the project's Kubernetes directory:

```bash
kubectl apply -f kubernetes/
```

### 3. Check Deployments

```bash
kubectl get deployments
```

The Food Ordering application uses these deployments:

- `food-frontend`
- `food-backend`
- `food-mongo`

### 4. Check Pods

```bash
kubectl get pods
```

All application pods should reach the `Running` state.

### 5. Check Services

```bash
kubectl get services
```

The application uses the following services:

| Component | Service Name | Type | Port |
|---|---|---|---|
| Frontend | food-frontend | NodePort | 30080 |
| Backend | backend | ClusterIP | 5000 |
| MongoDB | mongo | ClusterIP | 27017 |

### 6. Access the Frontend

Run:

```bash
minikube service food-frontend --url
```

Open the URL returned by Minikube in your browser.

Keep the terminal running while using the service URL if Minikube creates a local tunnel.

## MongoDB Persistent Storage

MongoDB stores the order records in a database collection.

The Kubernetes deployment uses a PersistentVolumeClaim named:

```text
mongo-pvc
```

The PVC provides persistent storage for MongoDB.

This means database records can remain available even when the MongoDB pod is restarted, provided the persistent volume is retained.

Check persistent volume claims:

```bash
kubectl get pvc
```

Check MongoDB pod:

```bash
kubectl get pods
```

Inspect the MongoDB deployment:

```bash
kubectl describe deployment food-mongo
```

## Application Verification

The application was tested after deployment.

| Test | Expected Result |
|---|---|
| Frontend deployment | Running |
| Backend deployment | Running |
| MongoDB deployment | Running |
| Frontend access | Opens in browser |
| Food item display | Food items displayed |
| Place order | Order submitted successfully |
| MongoDB storage | Order record saved |
| Database verification | Order record retrieved |

### Verify MongoDB Orders

Connect to the MongoDB pod:

```bash
kubectl exec deployment/food-mongo -- mongosh
```

Select the database and inspect the orders collection:

```javascript
show dbs
```

Select the appropriate application database and run:

```javascript
show collections
```

Then inspect the orders collection:

```javascript
db.orders.find().pretty()
```

The database and collection names should match the configuration used by the application.

## Useful Kubernetes Commands

Check all resources:

```bash
kubectl get all
```

Check pods:

```bash
kubectl get pods
```

Check deployments:

```bash
kubectl get deployments
```

Check services:

```bash
kubectl get services
```

Check persistent storage:

```bash
kubectl get pvc
```

View backend logs:

```bash
kubectl logs deployment/food-backend
```

View frontend logs:

```bash
kubectl logs deployment/food-frontend
```

View MongoDB logs:

```bash
kubectl logs deployment/food-mongo
```

Restart the application deployments:

```bash
kubectl rollout restart deployment/food-frontend

kubectl rollout restart deployment/food-backend
```

Check deployment status:

```bash
kubectl rollout status deployment/food-frontend

kubectl rollout status deployment/food-backend
```

## Troubleshooting

**1. Docker daemon is not running**

Start Docker Desktop and wait until the Docker Engine is running.

Check:

```bash
docker info
```

**2. Minikube is not running**

```bash
minikube start --driver=docker
```

**3. Pods are not starting**

```bash
kubectl get pods
kubectl describe pods
```

**4. Backend connection problems**

```bash
kubectl logs deployment/food-backend
```

Check the MongoDB connection configuration and Kubernetes service name.

**5. Frontend is not accessible**

```bash
kubectl get services
minikube service food-frontend --url
```

**6. MongoDB data verification**

```bash
kubectl exec deployment/food-mongo -- mongosh
```

Check the configured database and orders collection.

## Learning Outcomes

This project demonstrates practical experience with:

- Full-stack application development.
- React frontend development.
- Flask REST API development.
- MongoDB database integration.
- Docker image creation and containerization.
- Docker Compose orchestration.
- Publishing images to Docker Hub.
- Kubernetes Deployments and Services.
- Minikube-based application deployment.
- Persistent storage using Kubernetes PVCs.
- Application testing and deployment verification.

## Author

**Gontla Santhosh Kumar**

GitHub: [Santhoshkumargontla](https://github.com/Santhoshkumargontla)

Project Repository: [Food Ordering Application](https://github.com/Santhoshkumargontla/Food-Order)
