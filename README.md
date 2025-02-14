# FastAPI Book Management API

## Overview

This project is a RESTful API built with FastAPI for managing a book collection. It provides comprehensive CRUD operations for books with proper error handling, input validation, and auto-generated documentation.


## Features

The application features include:  
- 📚 Comprehensive book management (CRUD operations)  
- ✅ Input validation with Pydantic models  
- 🔍 Enum-based genre classification  
- 🧪 Complete test coverage using pytest  
- 📝 Auto-generated API documentation (Swagger UI and ReDoc)  
- 🔒 CORS middleware enabled  
- 🚀 CI/CD pipelines using GitHub Actions  
- ☁️ Automated deployment via Render  
- (Optional) 🔀 Nginx reverse proxy configuration on an AWS EC2 instance for advanced setups


## Project Structure

fastapi-book-project/
├── LICENSE
├── README.md
├── api
│   ├── __init__.py
│   ├── db/
│   │   ├── __init__.py
│   │   └── schemas.py
│   ├── router.py
│   └── routes/
│       ├── __init__.py
│       └── books.py
├── core/
│   ├── __init__.py
│   └── config.py
├── main.py
├── requirements.txt
├── run.py
└── tests/
    ├── __init__.py
    └── test_books.py

Note: The virtual environment directory ('.git', `venv/`) is excluded from version control.


## Technologies Used

- Python 3.12
- FastAPI
- Pydantic
- pytest
- uvicorn

## Installation

1. Clone the repository:

```bash
git clone https://github.com/hng12-devbotops/fastapi-book-project.git
cd fastapi-book-project
```

2. Create a virtual environment:

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Running the Application

1. Start the server:

```bash
uvicorn main:app
```

2. Access the API documentation:

- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

## API Endpoints

### Books

- `GET /api/v1/books/` - Get all books
- `GET /api/v1/books/{book_id}` - Get a specific book
- `POST /api/v1/books/` - Create a new book
- `PUT /api/v1/books/{book_id}` - Update a book
- `DELETE /api/v1/books/{book_id}` - Delete a book

### Health Check

- `GET /healthcheck` - Check API status

## Book Schema

```json
{
  "id": 1,
  "title": "Book Title",
  "author": "Author Name",
  "publication_year": 2024,
  "genre": "Fantasy"
}
```

Available genres:

- Science Fiction
- Fantasy
- Horror
- Mystery
- Romance
- Thriller

## Running Tests

```bash
pytest
```

## Error Handling

The API includes proper error handling for:

- Non-existent books
- Invalid book IDs
- Invalid genre types
- Malformed requests

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## CI/CD Pipelines

This project uses GitHub Actions for both Continuous Integration (CI) and Continuous Deployment (CD).
# CI Pipeline
Create a file at .github/workflows/ci.yml
# CD Pipeline
Create a file at .github/workflows/cd.yml

# GitHub Secrets Setup:
In your repository under Settings → Secrets and variables → Actions, add:

RENDER_API_KEY: Your Render API key
RENDER_SERVICE_ID: Your Render service ID

## Deployment
# Using Render

Render automatically deploys your application when changes are merged into the main branch. Configure your Render service with:

Build Command:
'pip install -r requirements.txt'
Start Command:
'uvicorn main:app --host 0.0.0.0 --port $PORT'

# Deployed API is available at:
'https://fastapi-book-project-avj3.onrender.com'

# Using an AWS EC2 Instance with Nginx as a Reverse Proxy
configure an AWS EC2 instance with Nginx as a reverse proxy.
# Provision an EC2 Instance

    Public IPv4 Address: 13.61.175.50
    Public IPv4 DNS: 'ec2-13-61-175-50.eu-north-1.compute.amazonaws.com'
    Connect using SSH:
    'ssh -i "instance2.pem" ubuntu@ec2-13-61-175-50.eu-north-1.compute.amazonaws.com'

# Install and Configure Nginx
    Install Nginx:
    'sudo apt update'
    'sudo apt install nginx -y'

# Nginx configuraation file at:
'/etc/nginx/sites-available/fastapi-proxy'

"server {
    listen 80;
    server_name ec2-13-61-175-50.eu-north-1.compute.amazonaws.com;

    location / {
        resolver 8.8.8.8 8.8.4.4 valid=300s;
        proxy_pass https://fastapi-book-project-avj3.onrender.com$request_uri;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        # Use Render's domain for SNI and SSL handshake
        proxy_set_header Host fastapi-book-project-avj3.onrender.com;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_ssl_server_name on;
        proxy_ssl_name fastapi-book-project-avj3.onrender.com;
        proxy_ssl_verify off;
        # Optionally, rewrite upstream redirects so they use your EC2 domain:
        proxy_redirect https://fastapi-book-project-avj3.onrender.com/ http://$host/;
    }
}"


# Test and reload Nginx:
'sudo nginx -t'
'sudo systemctl reload nginx'
# Access API via:
'http://ec2-13-61-175-50.eu-north-1.compute.amazonaws.com/api/v1/books/'

## Repository Access Requirement

# hng12-devbot GitHub account as a collaborator:

Navigate to your repository’s Settings.
Click on "Manage Access" (or "Collaborators & teams").
Invite the user hng12-devbot.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support, please open an issue in the GitHub repository.
