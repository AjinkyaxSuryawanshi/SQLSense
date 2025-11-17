# SQLSense - Natural Language to SQL Generator Backend API

SQLSense is a Flask-based REST API that converts natural language queries into SQL statements and generates database schemas from descriptions using AI.

## 🚀 Live Demo
Click the button to open the running app:  
[Open SQL Sense (Live)](https://sql-sense-omega.vercel.app/

## Features

- 🔤 **Natural Language to SQL**: Convert plain English queries to SQL statements.
- 🏗️ **Schema Generation**: Generate database schemas from natural language descriptions.
- 📊 **Multi-Database Support**: PostgreSQL, MySQL, and SQLite compatibility.
- 💾 **Query History**: Track and manage generated queries.
- 🗂️ **Schema Versioning**: Version control for database schemas.
- 🤖 **AI Assistant**: Interactive chat for SQL and database help.
- 🔌 **REST API**: Clean, well-documented API endpoints.

## Quick Start

### 1. Environment Setup

Copy the `.env` file and configure your settings:

```bash
cp .env .env.local
```

Edit `.env.local` with your values:

```bash
# Database Configuration
DATABASE_URL=postgresql://username:password@localhost:5432/sqlsense_db

# Session Security
SESSION_SECRET=your-secret-key-here-change-in-production

# OpenRouter API Configuration
OPENROUTER_API_KEY=your-openrouter-api-key-here

# Application Settings
FLASK_ENV=development
FLASK_DEBUG=True
```

### 2. Database Setup

**PostgreSQL (Recommended)**:
```bash
createdb sqlsense_db
```

**SQLite (Development)**:
```bash
# No setup needed - SQLite file will be created automatically
DATABASE_URL=sqlite:///sqlsense.db
```

### 3. OpenRouter API Key

1. Go to [OpenRouter.ai](https://openrouter.ai/)
2. Create an account
3. Get your API key from the dashboard
4. Add it to your `.env` file

### 4. Run the Application

```bash
# Install dependencies (if not using Replit)
pip install -r requirements-dev.txt

# Run the application
python main.py
```

Visit `http://localhost:5000` to see the API documentation.

## Project Structure

```
sqlsense/
├── app.py                 # Flask application factory
├── main.py               # Application entry point
├── config.py             # Configuration settings
├── models.py             # Database models
├── routes.py             # API routes and handlers
├── services/
│   ├── sql_generator.py  # SQL generation service
│   └── schema_generator.py # Schema generation service

├── .env                  # Environment variables template
├── requirements-dev.txt  # Development dependencies
└── README.md            # This file
```

## API Endpoints

### Root Endpoint
```bash
GET /
```
Returns API documentation and available endpoints.

### Health Check
```bash
GET /api/health
```
Returns API health status.

### SQL Generation
```bash
POST /api/generate-sql
Content-Type: application/json

{
  "prompt": "Show me all customers who made purchases last month",
  "context": "Optional database schema context",
  "database_type": "postgresql"
}
```
**Response:**
```json
{
  "sql_query": "SELECT * FROM customers WHERE...",
  "explanation": "Query explanation",
  "database_type": "postgresql",
  "model_used": "moonshotai/kimi-k2:free",
  "tables_involved": ["customers", "orders"]
}
```

### Schema Generation
```bash
POST /api/generate-schema
Content-Type: application/json

{
  "name": "E-commerce Database",
  "description": "Database for online store with users, products, orders",
  "database_type": "postgresql"
}
```
**Response:**
```json
{
  "schema": "CREATE TABLE users...",
  "explanation": "Schema explanation",
  "tables": [
    {
      "name": "users",
      "columns": ["id", "name", "email"]
    }
  ],
  "database_type": "postgresql",
  "model_used": "moonshotai/kimi-k2:free"
}
```

### Chat Assistant
```bash
POST /api/chat
Content-Type: application/json

{
  "message": "How do I optimize this query?",
  "type": "general"
}
```
**Response:**
```json
{
  "response": "To optimize your query, consider..."
}
```

### History & Data Management
```bash
GET /api/history              # Get query history (paginated)
GET /api/schema-versions      # Get schema versions
GET /api/chat/history         # Get chat history
POST /api/save               # Save/update query or schema metadata
```

**Query History Response:**
```json
{
  "queries": [...],
  "total": 50,
  "pages": 5,
  "current_page": 1
}
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_URL` | Database connection string | `sqlite:///sqlsense.db` |
| `SESSION_SECRET` | Flask session secret key | `dev-secret-key` |
| `OPENROUTER_API_KEY` | OpenRouter API key | Required |
| `FLASK_ENV` | Flask environment | `development` |
| `FLASK_DEBUG` | Debug mode | `True` |

### Free AI Models

The application uses OpenRouter's free AI models:
- `moonshotai/kimi-k2:free` (Default)
- `cognitivecomputations/dolphin-mistral-24b-venice-edition:free`
- `google/gemma-3n-e2b-it:free`
- `tencent/hunyuan-a13b-instruct:free`

## Development

### Local Development

```bash
# Clone the repository
git clone <repository-url>
cd sqlsense

# Set up virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements-dev.txt

# Set up environment
cp .env .env.local
# Edit .env.local with your settings

# Run the application
python main.py
```

### Database Migrations

The application automatically creates tables on startup. For production deployments, consider using proper migration tools like Alembic.

### Testing

```bash
# Run tests (when available)
pytest tests/

# Run with coverage
pytest --cov=. tests/
```

## Deployment

### Replit Deployment

This project is optimized for Replit deployment:

1. Fork the project on Replit
2. Add your `OPENROUTER_API_KEY` to Replit Secrets
3. Run the application

### Traditional Deployment

For platforms like Heroku, Railway, or Fly.io:

1. Set environment variables
2. Configure PostgreSQL database
3. Use `gunicorn` for production server:

```bash
gunicorn --bind 0.0.0.0:5000 --reuse-port --reload main:app
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.

## Support

For issues and questions:
- Check the [Issues](https://github.com/your-repo/sqlsense/issues) page
- Create a new issue if needed
- Contact the maintainers

---

Built with ❤️ using Flask, OpenRouter AI, and modern web technologies.
