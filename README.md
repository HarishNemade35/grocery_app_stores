# Online Grocery Shopping Backend


# Overview
This project provides a backend service for an online grocery shopping platform, focusing on the order management system. The API allows users to place, view, and update orders, as well as apply discounts through offer codes. It also includes features for user and owner authentication, and managing products within the grocery store.

The backend is built with FastAPI and uses PostgreSQL as the database.

Features
User Registration and Login: Allows users to register and log in to place orders.
Owner Registration and Login: Allows owners to manage products.
Product Management: Owners can create, view, and delete products.
Create Order: Place a new order by selecting a product, quantity, and optionally applying an offer code.
Get Order by ID: Retrieve the details of an order by its unique ID.
Update Order Status: Modify the status of an order.
Apply Offer: Apply a discount offer to an order.
Requirements
Python 3.7+
PostgreSQL for database
FastAPI for API development
Uvicorn for serving the application
Docker for containerization
Tech Stack
Backend Framework: FastAPI
Database: PostgreSQL
Authentication: JWT (JSON Web Tokens)
Containerization: Docker
Setup and Installation
Step 1: Clone the Repository
bash
Copy code
git clone <repository_url>
cd <project_directory>
Step 2: Install Dependencies
Create a virtual environment and install the dependencies listed in requirements.txt:

bash
Copy code
python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt
Step 3: Set Up the Database
Create the PostgreSQL database if it's not already set up.

bash
Copy code
# Create database (adjust as needed)
psql -U postgres -c "CREATE DATABASE grocery_db;"
Make sure to set the connection URL in your .env file for database access:

bash
Copy code
DATABASE_URL=postgresql://user:password@localhost/grocery_db
Step 4: Run Migrations
Use SQLAlchemy to create the necessary database tables:

bash
Copy code
# Run the database migrations
uvicorn app.main:app --reload
Step 5: Start the Application
Run the FastAPI application with Uvicorn:

bash
Copy code
uvicorn app.main:app --host 0.0.0.0 --port 8002 --reload
Step 6: Access the API
Once the server is running, access the API documentation at:

bash
Copy code
http://localhost:8002/docs
This interactive documentation will allow you to test the API endpoints directly.

File Structure
Here's the file structure of the project


![file str](https://github.com/user-attachments/assets/75506de3-ba15-4f1f-8190-75c453087f36)




# API Endpoints
![front screen](https://github.com/user-attachments/assets/79586024-0eec-4e43-a56e-aa586d363d6d)

![fastapi screen](https://github.com/user-attachments/assets/203c4468-cf87-48e3-98db-417a26eb3694)


# User Endpoints
# 1. User Registration
POST /user/register

Request Body (JSON):
json
Copy code
{
  "username": "john_doe",
  "password": "password123"
}

 Response:
json
Copy code
{
  "id": 1,
  "username": "john_doe"
}
# 2. User Login
POST /user/login

Request Body (JSON):
json
Copy code
{
  "username": "john_doe",
  "password": "password123"
}
Response:
json
Copy code
{
  "access_token": "JWT_TOKEN",
  "token_type": "bearer"
}

# Owner Endpoints
# 3. Owner Registration
POST /owner/register

Request Body (JSON):
json
Copy code
{
  "ownername": "store_owner",
  "password": "password123"
}
Response:
json
Copy code
{
  "id": 1,
  "ownername": "store_owner"
}

# 4. Owner Login
POST /owner/login

Request Body (JSON):
json
Copy code
{
  "ownername": "store_owner",
  "password": "password123"
}
Response:
json
Copy code
{
  "access_token": "JWT_TOKEN",
  "token_type": "bearer"
}
# Product Endpoints
# 5. Create Product (Owner only)
POST /product

Request Body (JSON):
json
Copy code
{
  "name": "Apple",
  "price": 100,
  "stock": 50
}
Response:
json
Copy code
{
  "id": 1,
  "name": "Apple",
  "price": 100,
  "stock": 50
}

# 6. Get All Products
GET /product/get_product

Response:
json
Copy code
[
  {
    "id": 1,
    "name": "Apple",
    "price": 100,
    "stock": 50
  },
  {
    "id": 2,
    "name": "Banana",
    "price": 30,
    "stock": 100
  }
]

# 7. Delete Product (Owner only)
DELETE /product/{product_id}

Response:
json
Copy code
{
  "message": "Product successfully deleted"
}
Order Endpoints
# 8. Create Order
POST /order

Request Body (JSON):
json
Copy code
{
  "product_id": 1,
  "quantity": 2,
  "offer_code": "DISCOUNT10"
}
Response:
json
Copy code
{
  "order_id": 1,
  "user_id": 1,
  "product_id": 1,
  "total_amount": 200,
  "discount_amount": 20,
  "final_amount": 180,
  "created_at": "2024-12-13T10:00:00Z"
}
# 9. Get Order by ID
GET /order/{order_id}

Response:
json
Copy code
{
  "order_id": 1,
  "user_id": 1,
  "product_id": 1,
  "total_amount": 200,
  "discount_amount": 20,
  "final_amount": 180,
  "created_at": "2024-12-13T10:00:00Z"
}
# 10. Update Order Status
PUT /order/{order_id}

Request Body (JSON):
json
Copy code
{
  "status": "shipped"
}
Response:
json
Copy code
{
  "order_id": 1,
  "status": "shipped"
}
# 11. Apply Offer
POST /offer/{offer_code}/apply

Request Body (JSON):
json
Copy code
{
  "order_id": 1,
  "offer_code": "DISCOUNT10"
}
Response:
json
Copy code
{
  "discount_amount": 20,
  "final_amount": 180
}


# Validation and Business Logic
Orders cannot be placed on public holidays or Sundays.
The minimum order amount is ₹99.
The maximum order amount is ₹4,999.
Products ordered must be in stock.
Offers are validated based on expiry, usage, and applicability to the product.
Each offer can only be claimed once by a user.


# Authentication
The application uses JWT (JSON Web Tokens) for user authentication.

To login, send a POST request to /user/login or /owner/login with the respective credentials.
The response will contain a token that should be included in the Authorization header for subsequent requests.


# Docker Setup (Optional)
![image](https://github.com/user-attachments/assets/d589ba21-5256-42ea-bce8-3b8e819115d9)

You can run the backend service with Docker by using the following steps:

Step 1: Build the Docker Image
bash
Copy code
docker build -t grocery_backend .
Step 2: Run Docker Compose
Ensure you have Docker Compose installed, then start the backend and database service:

bash
Copy code
docker-compose up --build
This will start both the FastAPI backend and a PostgreSQL container.

# Testing
Unit and integration tests are crucial for ensuring the functionality and reliability of the application. Add your tests in the tests/ directory. You can use pytest to run the tests:

bash
Copy code
pytest
## Conclusion
This backend API provides a full-featured solution for managing users, orders, products, and offers in an online grocery store. It is built with FastAPI, PostgreSQL, and JWT for secure authentication. You can further extend it by adding features like product reviews, inventory management, and user profiles.
