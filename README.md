# FastAPI PostgreSQL Application

A modern, production-ready FastAPI application with PostgreSQL database integration, featuring user authentication, product management, and a React frontend for data visualization.

## 📋 Overview

This application demonstrates best practices in building RESTful APIs with FastAPI and PostgreSQL. It includes user authentication with JWT tokens, database operations with SQLAlchemy ORM (async), and a React-based frontend for displaying top products by category.

## ✨ Key Features

- **User Authentication**: JWT-based authentication with signup, login, and token refresh
- **Product Management**: CRUD operations for products with category-based analytics
- **Database Operations**: Async SQLAlchemy ORM with PostgreSQL
- **Data Analytics**: Query top products by category with revenue calculations
- **Fake Data Generation**: Built-in data generator using Faker library
- **React Frontend**: Interactive UI to display product analytics
- **Docker Support**: Containerized deployment with Docker Compose
- **CORS Enabled**: Cross-origin resource sharing for frontend integration

## 🏗️ Project Structure

```
fastapi-psql-app/
├── database/
│   └── session.py              # Database connection and session management
├── models/
│   ├── __init__.py
│   ├── pydantic_models.py      # Pydantic models for request/response validation
│   └── schema.py               # SQLAlchemy database models
├── routes/
│   ├── __init__.py
│   ├── login.py                # Authentication endpoints (signup, login, token)
│   ├── populate_database.py    # Database population endpoints
│   └── products.py             # Product-related endpoints
├── utils/
│   ├── __init__.py
│   └── user_utils.py           # JWT token and password utilities
├── top-products/               # React frontend application
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── README.md
├── main.py                     # FastAPI application entry point
├── settings.py                 # Configuration management with Pydantic settings
├── requirements.txt            # Python dependencies
├── dockerfile                  # Docker configuration for FastAPI app
├── compose.yaml                # Docker Compose configuration
├── generate_data.py            # Script to generate fake product data CSV
├── fake_product_data.csv       # Generated product data
└── top5_customers_categories.sql  # SQL query for customer analytics
```

## 🛠️ Technology Stack

### Backend
- **FastAPI**: Modern, high-performance web framework
- **PostgreSQL**: Relational database
- **SQLAlchemy**: ORM with async support
- **Pydantic**: Data validation and settings management
- **asyncpg**: Async PostgreSQL driver
- **python-jose**: JWT token implementation
- **passlib & bcrypt**: Password hashing
- **Faker**: Fake data generation
- **Pandas**: Data manipulation for CSV operations

### Frontend
- **React 18**: UI library
- **Create React App**: Build tooling

### DevOps
- **Docker**: Containerization
- **Docker Compose**: Multi-container orchestration
- **Uvicorn**: ASGI server

## 📋 Prerequisites

- Python 3.10 or higher
- PostgreSQL 12 or higher (or use Docker)
- Node.js 16+ and npm (for frontend)
- Docker and Docker Compose (optional, for containerized deployment)

## 🚀 Setup and Installation

### 1. Clone the Repository

```bash
git clone https://github.com/deepanshu17/fastapi-psql-app.git
cd fastapi-psql-app
```

### 2. Environment Configuration

Create a `.env` file in the root directory with the following variables:

```env
# Database Configuration
POSTGRES_USER=your_db_user
POSTGRES_PASSWORD=your_db_password
POSTGRES_DB=your_database_name
POSTGRES_SERVICE=localhost
POSTGRES_PORT=5432
```

**Example `.env` for local development:**
```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=fastapi_db
POSTGRES_SERVICE=localhost
POSTGRES_PORT=5432
```

### 3. Backend Setup

#### Option A: Local Development

1. **Create a virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

3. **Ensure PostgreSQL is running** and the database specified in `.env` exists:
```bash
# Using psql command-line tool
createdb fastapi_db
```

4. **Run the FastAPI application:**
```bash
uvicorn main:app --host 0.0.0.0 --port 8001 --reload
```

The API will be available at `http://localhost:8001`

#### Option B: Docker Deployment

1. **Build and run with Docker Compose:**
```bash
# Create external network (if not exists)
docker network create demo-network

# Build and start the container
docker compose up --build
```

The API will be available at `http://localhost:8001`

### 4. Frontend Setup

1. **Navigate to the frontend directory:**
```bash
cd top-products
```

2. **Install dependencies:**
```bash
npm install
```

3. **Start the development server:**
```bash
npm start
```

The React app will be available at `http://localhost:3000`

## 📚 API Documentation

Once the application is running, you can access:

- **Swagger UI**: `http://localhost:8001/docs`
- **ReDoc**: `http://localhost:8001/redoc`

### Main Endpoints

#### Authentication

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| POST | `/signup` | Create new user account | No |
| POST | `/login` | Login with username/password | No |
| POST | `/token` | OAuth2 token endpoint (for Swagger UI) | No |
| GET | `/users/me` | Get current user info | Yes |

#### Products

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| GET | `/products/top-product` | Get top product per category with revenue | No |

#### Database Management

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| PUT | `/database/` | Populate DB with fake relational data | No |
| PUT | `/database/custom` | Populate DB with CSV product data | No |

## 💾 Database Schema

### Users Table
- `id`: Integer (Primary Key)
- `username`: String (Unique)
- `hashed_password`: String

### Customers Table
- `customer_id`: UUID (Primary Key)
- `customer_name`: String
- `email`: String
- `signup_date`: Date

### Products Table
- `product_id`: UUID (Primary Key)
- `product_name`: String
- `category`: String

### Custom Products Table
- `product_id`: Integer (Primary Key)
- `product_name`: String
- `category`: String
- `price`: Float
- `quantity_sold`: Integer
- `rating`: Float
- `review_count`: Integer

### Orders Table
- `order_id`: UUID (Primary Key)
- `customer_id`: UUID
- `order_date`: Date
- `total_amount`: Decimal

### Order Items Table
- `order_item_id`: UUID (Primary Key)
- `order_id`: UUID
- `product_id`: UUID
- `quantity`: Integer
- `price_per_unit`: Decimal


## 🔧 Development Workflow

### Generate Fake Product Data

To generate a CSV file with fake product data:

```bash
python generate_data.py
```

This creates `fake_product_data.csv` with 50,000 product records across 6 categories.

### Populate Database

**Option 1: Populate with relational data (customers, orders, products)**
```bash
curl -X PUT "http://localhost:8001/database/" \
  -H "Content-Type: application/json" \
  -d '{"customer_count": 500, "product_count": 100, "order_count": 1000}'
```

**Option 2: Populate with custom product data from CSV**
```bash
curl -X PUT "http://localhost:8001/database/custom" \
  -H "Content-Type: application/json" \
  -d '{}'
```

### Run with Auto-reload (Development)

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8001
```

## 🐳 Docker Commands

### Build the image
```bash
docker build -t fastapi_app_image .
```

### Run standalone container
```bash
docker run -p 8001:8001 --env-file .env fastapi_app_image
```

### Using Docker Compose
```bash
# Start services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down

# Rebuild and restart
docker compose up --build
```

## 🔐 Security Notes

- The application uses JWT tokens for authentication
- Passwords are hashed using bcrypt
- **Important**: Change the `SECRET_KEY` in `utils/user_utils.py` for production
- Update CORS settings in `main.py` to restrict origins in production
- Never commit `.env` files with sensitive credentials

## 🧪 Testing

The API can be tested using:
- **Swagger UI**: `http://localhost:8001/docs` (interactive API testing)
- **Postman**: Import endpoints from the Swagger JSON
- **curl**: Command-line HTTP requests (examples above)

## 📝 Additional Information

### Database Queries

The repository includes a SQL file `top5_customers_categories.sql` that demonstrates:
- Finding top 5 customers by spending in the last year
- Identifying their most purchased product category

### Frontend Features

The React application (`top-products/`) provides:
- Visual display of top products by category
- Revenue information per category
- Responsive UI design

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is available for educational and demonstration purposes.

## 👤 Author

**Deepanshu**
- GitHub: [@deepanshu17](https://github.com/deepanshu17)

## 🙏 Acknowledgments

- FastAPI framework for excellent documentation and developer experience
- SQLAlchemy for powerful ORM capabilities
- Faker library for test data generation
