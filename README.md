# Food-Order
# Online Food Ordering Application

A containerized online food ordering application built using **React, Flask, MongoDB, Docker, and Docker Compose**. Users can browse food items, add items to their cart, and place orders. The backend processes orders and stores them in MongoDB.

## Features

- Browse a food menu with item names, prices, categories, and descriptions.
- Search and filter food items.
- Add food items to the cart and update quantities.
- Place food orders through a REST API.
- Calculate order totals on the backend.
- Store orders persistently in MongoDB.
- Run the frontend, backend, and database using Docker Compose.
- Use Nginx to serve the React frontend and proxy API requests to Flask.

## Tech Stack

| Technology | Purpose |
|---|---|
| React | Frontend user interface |
| Vite | Frontend development and build tool |
| Python | Backend programming language |
| Flask | REST API |
| Flask-CORS | Cross-origin request support |
| PyMongo | MongoDB integration |
| MongoDB | Food and order data storage |
| Nginx | Frontend web server and API reverse proxy |
| Docker | Application containerization |
| Docker Compose | Multi-container orchestration |

## Project Structure

```text
food-ordering/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── App.jsx
│   │   ├── App.css
│   │   └── index.css
│   ├── public/
│   ├── package.json
│   ├── package-lock.json
│   ├── nginx.conf
│   └── Dockerfile
└── docker-compose.yml
```

## Prerequisites

Install the following software:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Git (optional, for version control)

Docker Desktop must be running before starting the application.

## Getting Started

### 1. Clone the repository

If you have uploaded this project to GitHub:

```bash
git clone <your-repository-url>
cd food-ordering
```

Alternatively, open PowerShell in your existing project directory:

```powershell
cd C:\Users\santh\food-ordering
```

### 2. Build and start the application

Run:

```bash
docker compose up --build -d
```

Docker Compose builds the frontend and backend images, starts MongoDB, and launches the services.

### 3. Check the containers

```bash
docker compose ps
```

The `frontend`, `backend`, and `mongo` services should be running. MongoDB should report a healthy status.

### 4. Open the application

| Service | URL |
|---|---|
| Frontend | http://localhost:8080 |
| Backend health check | http://localhost:5000/api/health |
| Food menu API through Nginx | http://localhost:8080/api/foods |

The health endpoint should report a successful database connection.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/health` | Checks backend and database connectivity |
| GET | `/api/foods` | Retrieves the food menu |
| POST | `/api/orders` | Creates a food order |

### Example: Retrieve the food menu

```bash
curl http://localhost:5000/api/foods
```

### Example: Place an order

Replace the example food ID with a valid ID returned by the food menu API.

```json
{
  "items": [
    {
      "foodId": "YOUR_FOOD_ID",
      "quantity": 1
    }
  ]
}
```

Send this JSON as a POST request to:

```text
http://localhost:5000/api/orders
```

A successful request returns the order ID, total amount, and order status.

## Verify Orders in MongoDB

Check the number of saved orders:

```bash
docker compose exec mongo mongosh food_ordering --eval "db.orders.countDocuments()"
```

Display saved orders:

```bash
docker compose exec mongo mongosh food_ordering --eval "db.orders.find().pretty()"
```

The project was tested successfully by placing an order for one Classic Burger priced at ₹149 and verifying that the order was saved in MongoDB.

## Useful Docker Commands

**View running containers**

```bash
docker compose ps
```

**View application logs**

```bash
docker compose logs -f
```

**View backend logs**

```bash
docker compose logs --tail=50 backend
```

**Restart the application**

```bash
docker compose restart
```

**Stop the containers**

```bash
docker compose down
```

The named MongoDB volume preserves database data when containers are stopped or removed with `docker compose down`.

> Warning: `docker compose down -v` removes the named volumes and can delete the stored database data.

## Docker Architecture

```text
User's Browser
      |
      v
React Frontend + Nginx
      |
      | /api requests
      v
Flask Backend
      |
      | PyMongo
      v
MongoDB Database
      |
      v
Persistent Docker Volume
```

Docker Compose connects the services through its internal network. Nginx forwards frontend API requests to the Flask backend, and Flask communicates with MongoDB using the `MONGO_URI` environment variable.

## Learning Outcomes

This project demonstrates:

- Writing Dockerfiles for frontend and backend applications.
- Building Docker images.
- Running multiple containers with Docker Compose.
- Configuring communication between containers.
- Using environment variables for service configuration.
- Persisting data with Docker volumes.
- Connecting a REST API to MongoDB.
- Testing API responses and database persistence.
- Using Nginx as a reverse proxy.

## Future Improvements

- Use Gunicorn for production-oriented Flask serving.
- Add automated unit and integration tests.
- Add a CI/CD pipeline using GitHub Actions or Jenkins.
- Deploy the application to AWS.
- Add authentication and order history.

## Project Status

**Completed — Core Docker Compose workflow verified.**

The React frontend, Flask backend, and MongoDB database were started successfully. Food retrieval and order creation were tested, and a saved order was verified in MongoDB.

## License

This project is intended for learning and portfolio demonstration. Add a license file if you plan to distribute it publicly.
