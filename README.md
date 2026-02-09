# FastAPI Backend Template Application

A production-ready FastAPI backend template with PostgreSQL, Redis, JWT authentication, and Docker support.

## 🚀 Features

- **FastAPI Framework** - Modern, fast web framework for building APIs
- **PostgreSQL Database** - Robust relational database with async support
- **Redis Integration** - Caching and session management
- **JWT Authentication** - Secure token-based authentication
- **Docker Support** - Containerized deployment with Docker Compose
- **Database Migrations** - Alembic for database schema versioning
- **Repository Pattern** - Clean architecture with separation of concerns
- **Type Safety** - Pydantic models for request/response validation
- **Password Hashing** - Secure password storage with bcrypt

## 📋 Prerequisites

- Python 3.10 or higher
- Docker and Docker Compose (for containerized deployment)
- PostgreSQL 14+ (if running without Docker)
- Redis 7+ (if running without Docker)

## 🛠️ Tech Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL with SQLAlchemy (async)
- **Cache**: Redis
- **Authentication**: JWT (python-jose)
- **Password Hashing**: Passlib with bcrypt
- **Migrations**: Alembic
- **Server**: Uvicorn

## 📁 Project Structure

```
FastAPI-PostgreSQL-Redis-Docker-JWT-Template/
├── alembic/                    # Database migration files
│   └── versions/               # Migration versions
├── app/
│   ├── configs/                # Configuration modules
│   │   ├── configs.py          # App configuration
│   │   ├── hashing.py          # Password hashing utilities
│   │   ├── oauth2.py           # OAuth2 scheme configuration
│   │   └── token.py            # JWT token utilities
│   ├── db/                     # Database connections
│   │   ├── database.py         # PostgreSQL connection
│   │   └── redis_client.py     # Redis connection
│   ├── repository/             # Data access layer
│   │   └── userRepository.py   # User repository
│   ├── routers/                # API route handlers
│   │   ├── auth.py             # Authentication endpoints
│   │   └── user.py             # User management endpoints
│   ├── main.py                 # Application entry point
│   ├── models.py               # SQLAlchemy models
│   └── schemas.py              # Pydantic schemas
│
├── docker-compose.yml          # Docker Compose configuration
├── Dockerfile                  # Docker image definition
├── alembic.ini                 # Alembic configuration
├── requirements.txt            # Python dependencies
├── pyproject.toml              # Project metadata and dependencies
└── .env                        # Environment variables (create from .env.example)
```

## 🚀 Quick Start

### Option 1: Using Docker (Recommended)

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd FastAPI-PostgreSQL-Redis-Docker-JWT-Template
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and set your configuration values.

3. **Generate a secret key**
   ```bash
   openssl rand -hex 32
   ```
   Copy the output and set it as `SECRET_KEY` in your `.env` file.

4. **Start the services**
   ```bash
   docker-compose up -d
   ```

5. **Run database migrations**
   ```bash
   docker-compose exec app alembic upgrade head
   ```

6. **Access the application**
   - API: http://localhost:8000
   - Interactive API docs: http://localhost:8000/docs
   - Alternative API docs: http://localhost:8000/redoc

### Option 2: Local Development

1. **Clone the repository**
   ```bash
   git clone AbdullahAbdelaziz122/FastAPI-PostgreSQL-Redis-Docker-JWT-Template
   cd FastAPI-PostgreSQL-Redis-Docker-JWT-Template
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   # For development dependencies
   pip install -e ".[dev]"
   ```

4. **Setup PostgreSQL and Redis**
   - Install and start PostgreSQL
   - Install and start Redis
   - Create a database for the application

5. **Create environment file**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your local database credentials.

6. **Run database migrations**
   ```bash
   alembic upgrade head
   ```

7. **Start the development server**
   ```bash
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
   ```

## 🔧 Environment Configuration

All environment variables are documented in `.env.example`. Key variables include:

- `DATABASE_URL`: PostgreSQL connection string
- `REDIS_HOST`: Redis server host
- `REDIS_PORT`: Redis server port
- `SECRET_KEY`: JWT secret key (generate with `openssl rand -hex 32`)
- `ALGORITHM`: JWT algorithm (default: HS256)
- `ACCESS_TOKEN_EXPIRE_MINUTES`: Token expiration time
- `ALLOWED_ORIGINS`: CORS allowed origins

## 📊 Database Migrations

This project uses Alembic for database migrations.

### Create a new migration

```bash
# Auto-generate migration from model changes
alembic revision --autogenerate -m "description of changes"

# Create empty migration
alembic revision -m "description of changes"
```

### Apply migrations

```bash
# Upgrade to latest version
alembic upgrade head

# Upgrade to specific revision
alembic upgrade <revision_id>

# Downgrade one version
alembic downgrade -1

# Downgrade to specific revision
alembic downgrade <revision_id>
```

### View migration history

```bash
# Show current revision
alembic current

# Show migration history
alembic history

# Show pending migrations
alembic history --verbose
```

### Docker environment

```bash
# Run migrations in Docker
docker-compose exec app alembic upgrade head

# Create new migration in Docker
docker-compose exec app alembic revision --autogenerate -m "description"
```

## 📚 API Documentation

### Authentication Endpoints

#### Register a new user
```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "username": "username",
  "password": "secure_password"
}
```

**Response**: `201 Created`
```json
{
  "id": 1,
  "email": "user@example.com",
  "username": "username",
  "created_at": "2024-01-01T00:00:00"
}
```

#### Login
```http
POST /auth/login
Content-Type: application/x-www-form-urlencoded

username=user@example.com&password=secure_password
```

**Response**: `200 OK`
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### User Endpoints

#### Get current user
```http
GET /users/me
Authorization: Bearer <access_token>
```

**Response**: `200 OK`
```json
{
  "id": 1,
  "email": "user@example.com",
  "username": "username",
  "created_at": "2024-01-01T00:00:00"
}
```

#### Get user by ID
```http
GET /users/{user_id}
Authorization: Bearer <access_token>
```

**Response**: `200 OK`
```json
{
  "id": 1,
  "email": "user@example.com",
  "username": "username",
  "created_at": "2024-01-01T00:00:00"
}
```

### Interactive Documentation

Once the server is running, visit:
- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

These interfaces provide:
- Complete API endpoint documentation
- Request/response schemas
- Interactive API testing
- Authentication flow testing

## 🐳 Docker Deployment

### Build and Run

```bash
# Build and start all services
docker-compose up -d --build

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Stop and remove volumes
docker-compose down -v
```

### Service Configuration

The `docker-compose.yml` defines three services:
- **app**: FastAPI application
- **db**: PostgreSQL database
- **redis**: Redis cache

### Environment Variables in Docker

Environment variables can be set in:
1. `.env` file (recommended for local development)
2. `docker-compose.yml` environment section
3. Passed directly: `docker-compose up -e KEY=value`

### Production Deployment

For production deployments:

1. **Set secure environment variables**
   - Use strong `SECRET_KEY`
   - Set `DEBUG=False`
   - Configure specific `ALLOWED_ORIGINS`

2. **Use production-ready database**
   - Use managed PostgreSQL service or
   - Configure PostgreSQL with proper backups and replication

3. **Configure Redis persistence**
   - Enable RDB or AOF persistence
   - Set up Redis password authentication

4. **Use a reverse proxy**
   - Deploy behind Nginx or Traefik
   - Configure SSL/TLS certificates
   - Set up rate limiting

5. **Health checks and monitoring**
   - Implement health check endpoints
   - Configure logging and monitoring
   - Set up alerting

## 🔒 Security Best Practices

- **Never commit `.env` file** - It's in `.gitignore` by default
- **Use strong SECRET_KEY** - Generate with `openssl rand -hex 32`
- **Set specific CORS origins** - Don't use `["*"]` in production
- **Enable HTTPS** - Use TLS certificates in production
- **Validate input data** - Pydantic schemas handle this automatically
- **Rate limiting** - Consider implementing rate limiting for API endpoints
- **Update dependencies** - Regularly update packages for security patches

## 🧪 Testing

```bash
# Install dev dependencies
pip install -e ".[dev]"

# Run tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test file
pytest tests/test_auth.py
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Troubleshooting

### Database connection errors
- Verify PostgreSQL is running: `docker-compose ps`
- Check DATABASE_URL is correct in `.env`
- Ensure database exists and user has proper permissions

### Redis connection errors
- Verify Redis is running: `docker-compose ps`
- Check REDIS_HOST and REDIS_PORT in `.env`
- Ensure Redis is accessible from the application

### Migration errors
- Ensure database is running before migrations
- Check alembic.ini configuration
- Verify models are properly imported in `app/models.py`

### Docker build errors
- Clear Docker cache: `docker-compose build --no-cache`
- Remove old containers: `docker-compose down -v`
- Check Docker daemon is running

## 📧 Support

For issues, questions, or contributions, please open an issue on GitHub.

---

## Social Media
- Linkedin: [Linkedin](www.linkedin.com/in/abdullah-abdulaziz-klx)

**Built with ❤️ using FastAPI**
