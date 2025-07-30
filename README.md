# ContactDB API by Sir Lance

A RESTful API for managing contact information built with ASP.NET Core 9.0 and PostgreSQL. This API provides comprehensive CRUD operations for contact management with features like pagination, search, and bulk operations.

## 🚀 Features

- **CRUD Operations**: Full Create, Read, Update, Delete functionality for contacts
- **Search & Filter**: Search contacts by name or across all fields
- **Pagination**: Efficient pagination support for large datasets
- **Bulk Operations**: Upload multiple contacts at once
- **RESTful Design**: Follows REST API best practices
- **Swagger Documentation**: Interactive API documentation
- **CORS Support**: Cross-origin resource sharing enabled
- **PostgreSQL Integration**: Robust database connectivity with Npgsql

## 🏗️ Architecture

### Project Structure

```
API/
├── ContactModule/           # Contact domain module
│   ├── Contact.cs          # Contact entity model
│   ├── IContactRepository.cs # Repository interface
│   └── ContactRepository.cs # Repository implementation
├── Controllers/            # API controllers
│   └── ContactController.cs # Contact API endpoints
├── Main/                   # Core infrastructure
│   ├── BaseRepository.cs   # Base repository pattern
│   ├── MyCon.cs           # Database connection management
│   └── PaginationModel.cs # Pagination support
├── Program.cs              # Application entry point
└── API.csproj             # Project configuration
```

### Technology Stack

- **Framework**: ASP.NET Core 9.0
- **Database**: PostgreSQL with Npgsql
- **Authentication**: JWT Bearer (configured but not implemented)
- **Documentation**: Swagger/OpenAPI
- **JSON**: Newtonsoft.Json

## 📋 Prerequisites

- .NET 9.0 SDK
- PostgreSQL database
- Visual Studio 2022 or VS Code

## ⚙️ Setup & Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd API
```

### 2. Database Configuration

Update the connection string in `Main/MyCon.cs`:

```csharp
connectionString = "Host=your-host;" + 
    "Port=5432;" +
    "Database=your-database;" +
    "Username=your-username;" +
    "Password=your-password;" + 
    "SSL Mode=Require;" +
    "Trust Server Certificate=True;";
```

### 3. Database Schema

Create the contact table in your PostgreSQL database:

```sql
CREATE TABLE contact (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    gender VARCHAR(50),
    birthday DATE,
    address TEXT,
    contact_num VARCHAR(50)
);
```

### 4. Build and Run

```bash
# Restore dependencies
dotnet restore

# Build the project
dotnet build

# Run the application
dotnet run
```

The API will be available at `https://localhost:5193` (or the configured port).

## 📚 API Documentation

Once the application is running, you can access the interactive Swagger documentation at:
`https://localhost:5193/swagger`

## 🔌 API Endpoints

### Contact Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/contact` | Get all contacts |
| `GET` | `/api/contact/{id}` | Get contact by ID |
| `GET` | `/api/contact/search-by-name?name={name}` | Search contacts by name |
| `GET` | `/api/contact/search-all?query={query}` | Search across all fields |
| `GET` | `/api/contact/paginated?pageNumber={page}&pageSize={size}` | Get paginated contacts |
| `POST` | `/api/contact` | Create new contact |
| `PUT` | `/api/contact/{id}` | Update existing contact |
| `DELETE` | `/api/contact/{id}` | Delete contact by ID |
| `DELETE` | `/api/contact/delete-all` | Delete all contacts |
| `POST` | `/api/contact/bulk-upload` | Upload multiple contacts |

### Contact Model

```json
{
  "id": 1,
  "name": "John Doe",
  "gender": "Male",
  "birthday": "1990-01-01T00:00:00",
  "address": "123 Main St, City, State",
  "contactNum": "+1-555-123-4567"
}
```

### Pagination Response

```json
{
  "items": [...],
  "totalCount": 100,
  "pageSize": 25,
  "currentPage": 1,
  "totalPages": 4,
  "hasPreviousPage": false,
  "hasNextPage": true
}
```

## 🔧 Configuration

### Environment Variables

The application uses the following configuration in `appsettings.json`:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### CORS Configuration

CORS is configured to allow all origins, methods, and headers for development. For production, you should restrict this to specific domains.

## 🛠️ Development

### Project Structure Details

#### ContactModule
- **Contact.cs**: Entity model with data annotations for PostgreSQL mapping
- **IContactRepository.cs**: Interface defining all repository operations
- **ContactRepository.cs**: Implementation of contact data access layer

#### Controllers
- **ContactController.cs**: RESTful API endpoints with proper HTTP status codes and response types

#### Main
- **BaseRepository.cs**: Abstract base class providing common database operations
- **MyCon.cs**: Database connection management with error handling
- **PaginationModel.cs**: Generic pagination model for consistent API responses

### Key Design Patterns

1. **Repository Pattern**: Abstracts data access logic
2. **Dependency Injection**: Services registered in Program.cs
3. **Async/Await**: All database operations are asynchronous
4. **Generic Repository**: BaseRepository provides common CRUD operations

## 🔒 Security Considerations

- JWT Bearer authentication is configured but not implemented
- CORS is currently open for development
- Database connection uses SSL
- Parameterized queries prevent SQL injection

## 🚀 Deployment

### Docker (Recommended)

Create a `Dockerfile`:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY ["API.csproj", "./"]
RUN dotnet restore "API.csproj"
COPY . .
RUN dotnet build "API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "API.dll"]
```

### Azure App Service

1. Build the project: `dotnet publish -c Release`
2. Deploy to Azure App Service
3. Configure connection string in Application Settings

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📝 License

This project is licensed under the MIT License.

## 🆘 Support

For issues and questions:
1. Check the existing issues
2. Create a new issue with detailed information
3. Include error messages and steps to reproduce

---

**Note**: This API is designed for development and testing. For production use, ensure proper security measures, logging, and monitoring are implemented. 
