# PouchDB-CouchDB Demo Application

A demonstration application showing offline-first synchronization between PouchDB (client-side) and CouchDB (server-side).

## Overview

This project showcases a real-time synchronization pattern between browser-based applications and a server database using:

- **PouchDB**: Client-side NoSQL database for local data storage
- **CouchDB**: Server-side document database providing the sync endpoint
- **Nginx**: Reverse proxy for handling CORS and routing
- **Docker**: Container orchestration for easy setup

## Features

- 🔄 Real-time bidirectional sync between client and server
- 🔒 User authentication
- 📝 CRUD operations on documents
- 📱 Offline-first functionality
- 🕰️ Time-based filtering of documents

## Getting Started

### Prerequisites

- Docker and Docker Compose
- Web browser

### Installation

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/pouchdb-couchdb.git
   cd pouchdb-couchdb
   ```

2. Start the application:
   ```
   docker-compose up -d
   ```

3. Access the application:
   Open `http://localhost:8080` in your web browser

### Configuration

The default CouchDB credentials are:
- Username: `admin`
- Password: `admin`

These can be modified in the `docker-compose.yml` file.

## Architecture

- **Frontend**: HTML/JS application using PouchDB for local storage
- **Backend**: CouchDB database for server-side storage
- **Proxy**: Nginx handles routing and CORS issues
- **Networking**: Docker Compose manages container networking

## CouchDB Filters

This application uses CouchDB filter functions for efficient data synchronization.

### Filters Setup

A design document with filter functions is provided in `CouchDB/filters.json`:

```json
{
    "_id": "_design/filters",
    "filters": {
      "by_timestamp": "function(doc, req) { if (!doc.timestamp) return false; var since = req.query.since; return doc.timestamp > since; }"
    }
}
```

This filter function (`by_timestamp`) allows synchronization of only documents with timestamps newer than a specified value.

### How to Deploy Filters

The filters need to be uploaded to your CouchDB instance. You can do this by:

1. Accessing the CouchDB Fauxton UI at `http://localhost:8080/db/_utils/`
2. Creating a new design document
3. Pasting the contents of `filters.json`

Alternatively, you can use curl:

```
curl -X PUT http://admin:admin@localhost:8080/db/_design/filters \
     -H "Content-Type: application/json" \
     -d @CouchDB/filters.json
```

## Development

### Project Structure

- `frontend/`: Contains the HTML/JS application
- `nginx/`: Contains Nginx configuration
- `CouchDB/`: Contains CouchDB filter design documents and configuration
- `docker-compose.yml`: Defines services and their configuration

## License

[MIT License](LICENSE)