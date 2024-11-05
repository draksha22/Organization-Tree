# Organization Tree Backend

A backend solution for managing an organization tree, where the root node signifies the organization and child nodes represent locations, employees, and departments. This project is built using Node.js, Express, and Sequelize, and utilizes a PostgreSQL database.

## Features

- Create, update, delete, and retrieve nodes in an organizational hierarchy.
- Prevent cycles in the hierarchy by validating parent-child relationships.
- Assign colors to location and department nodes in a round-robin manner.
- Propagate colors to child nodes for a clear visual representation.
- Support for deep hierarchies, with efficient retrieval and updates.

---

## Table of Contents

- [Technologies](#technologies)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Database Setup](#database-setup)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)
- [Deployment](#deployment)
- [Testing](#testing)
- [License](#license)

---

## Technologies

- **Node.js**: JavaScript runtime for backend services.
- **Express**: Fast, minimalist web framework for Node.js.
- **Sequelize**: ORM for managing relational databases.
- **PostgreSQL**: Relational database for storing organizational data.
- **Docker**: Containerization for easy deployment.

---

## Project Structure

```plaintext
organization-tree/
├── config/
│   └── config.json         # Database configuration for Sequelize
├── models/
│   ├── index.js            # Sequelize initialization
│   └── Node.js             # Node model definition
├── controllers/
│   ├── nodeController.js   # Logic for API endpoints
│   └── colorUtils.js       # Color assignment utility
├── routes/
│   └── nodeRoutes.js       # API route definitions
├── app.js                  # Express app setup
├── server.js               # Server entry point
└── README.md               # Project documentation
```

---

## Getting Started

### Prerequisites

- **Node.js**: [Download Node.js](https://nodejs.org/en/download/)
- **PostgreSQL**: [Install PostgreSQL](https://www.postgresql.org/download/)

### Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/your-username/organization-tree.git
   cd organization-tree
   ```

2. **Install dependencies**:

   ```bash
   npm install
   ```

3. **Set up PostgreSQL database**:
   - Create a database named `organization_tree` or update the name in `config/config.json`.

4. **Run migrations** (Sequelize sync in this case):

   ```bash
   node models/index.js
   ```

5. **Start the server**:

   ```bash
   node server.js
   ```

---

## Database Setup

This project uses Sequelize ORM to interface with a PostgreSQL database. The database schema consists of a single `Nodes` table with the following fields:

- `NODE_ID` (Primary Key): Unique identifier for each node.
- `NODE_NAME`: Name of the node.
- `NODE_TYPE`: Type of node (location, employee, department).
- `NODE_COLOR`: Hex color code, assigned for location and department nodes.
- `PARENT_ID`: Foreign key linking to the parent node’s ID.

The table supports recursive relationships through the `PARENT_ID` column.

### Environment Variables

You may want to configure environment variables in a `.env` file. Set up the following (or directly edit `config/config.json`):

```plaintext
DB_USERNAME=your_db_username
DB_PASSWORD=your_db_password
DB_NAME=organization_tree
DB_HOST=127.0.0.1
DB_DIALECT=postgres
PORT=3000
```

---

## API Endpoints

### 1. Create Node

- **Endpoint**: `/api/nodes`
- **Method**: `POST`
- **Description**: Creates a new node in the organizational tree.
- **Request Body**:
  ```json
  {
    "nodeName": "string",
    "nodeType": "string (location | employee | department)",
    "parentId": "number (optional)"
  }
  ```
- **Response**:
  - `201 Created`: Returns the created node.
  - `400 Bad Request`: If a cycle is detected or invalid data is provided.

### 2. Update Node

- **Endpoint**: `/api/nodes/{nodeId}`
- **Method**: `PUT`
- **Description**: Updates a node’s parent or name, with options to manage child nodes.
- **Request Body**:
  ```json
  {
    "nodeName": "string (optional)",
    "parentId": "number (optional)",
    "option": "string (moveWithChildren | shiftChildrenUp)"
  }
  ```
- **Response**:
  - `200 OK`: Returns the updated node.
  - `400 Bad Request`: If a cycle is detected or invalid data is provided.

### 3. Delete Node

- **Endpoint**: `/api/nodes/{nodeId}`
- **Method**: `DELETE`
- **Description**: Deletes a node, with options to handle child nodes.
- **Request Body**:
  ```json
  {
    "option": "string (removeWithChildren | shiftChildrenUp)"
  }
  ```
- **Response**:
  - `200 OK`: Confirmation of node deletion.
  - `400 Bad Request`: If the node is not found or invalid data is provided.

### 4. Get Node Tree

- **Endpoint**: `/api/nodes/tree`
- **Method**: `GET`
- **Description**: Retrieves the entire organizational tree structure.
- **Response**:
  - `200 OK`: Returns the hierarchical structure with all nodes.

---

## Deployment

1. **Dockerize**:
   - Create a `Dockerfile` and `docker-compose.yml` for easy deployment in a containerized environment.
   
2. **Environment Variables**:
   - Ensure `.env` is properly configured.

3. **Deploy**:
   - You can deploy the container to any cloud provider or use a cloud service like Heroku, AWS, or Azure.

---

## Testing

Testing is essential to validate the functionality of each API endpoint and prevent regressions. Here are the types of tests recommended:

- **Unit Tests**: Test each controller function in isolation.
- **Integration Tests**: Test the endpoints using a tool like Postman or Jest to verify end-to-end functionality.

Example command to run tests (if using Jest):

```bash
npm test
```

---

## License

This project is licensed under the MIT License.

---

## Contact

For questions, reach out via email at [drakshakhan2211@gmail.com](mailto:drakshakhan2211@gmail.com).

--- 

This README covers setup, configuration, and usage instructions, providing a clear guide for other developers. Let me know if you'd like to add or clarify any specific details!
