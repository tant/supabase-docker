# Supabase Docker

A minimal, self-hosted Supabase setup using Docker Compose. This project provides a straightforward way to deploy the full Supabase stack on your own infrastructure.

## Overview

This repository contains the necessary Docker Compose files and configurations to run all the core Supabase services. It's designed for developers who want to self-host Supabase for development, testing, or production environments with full control over the underlying infrastructure.

## Features

- **Complete Supabase Stack:** Includes Postgres, Realtime, Storage, API Gateway, and more.
- **Multiple Configurations:** Supports base, development (`dev`), and S3 storage setups.
- **Serverless Functions:** Integrated support for Supabase Functions.
- **Easy Data Reset:** A simple script to reset your database and volumes.
- **Customizable:** Easily extend and customize the configuration via YAML files.

## Prerequisites

- **Docker:** Version 20.10 or later.
- **Docker Compose:** Version 1.29 or later.
- **Operating System:** macOS, Linux, or Windows.

## Getting Started

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/tant/supabase-docker.git
    cd supabase-docker
    ```

2.  **Create Environment File:**
    Create a `.env` file by copying the example variables from `docker-compose.yml` or `.env.sample`. This file will store your project's secrets and configuration.

3.  **Start the Services:**
    You can start the services using one of the following configurations:

    *   **Default:**
        ```bash
        docker compose up -d
        ```

    *   **With Development Helpers:**
        ```bash
        docker compose -f docker-compose.yml -f ./dev/docker-compose.dev.yml up -d
        ```

    *   **With S3 Storage:**
        ```bash
        docker compose -f docker-compose.yml -f docker-compose.s3.yml up -d
        ```

4.  **Access Supabase Studio:**
    Once the services are running, you can access the Supabase Studio at `http://localhost:3000`.

## Project Structure

```
.
├── docker-compose.yml      # Main services configuration
├── docker-compose.s3.yml   # S3 storage configuration override
├── .env.sample             # Sample environment variables
├── reset.sh                # Script to reset data
├── dev/
│   ├── data.sql            # Seed data for development
│   └── docker-compose.dev.yml # Development-specific services
└── volumes/                # Persisted data (logs, db, etc.)
    ├── api/
    ├── db/
    ├── functions/
    └── ...
```

## Service Reference

The following services are included in the default `docker-compose.yml`:

| Service     | Description                               |
|-------------|-------------------------------------------|
| `studio`    | The Supabase management UI.               |
| `kong`      | API Gateway for all services.             |
| `auth`      | Handles user authentication.              |
| `rest`      | Provides a RESTful API for Postgres.      |
| `realtime`  | Realtime messaging and data sync.         |
| `storage`   | Manages file storage and access.          |
| `imgproxy`  | On-the-fly image processing.              |
| `meta`      | Manages Postgres metadata.                |
| `functions` | Executes serverless functions.            |
| `analytics` | Handles logging and analytics.            |
| `db`        | The core PostgreSQL database.             |
| `vector`    | Logging and metrics agent.                |
| `supavisor` | Connection pooler for Postgres.           |

## Common Commands

- **Start Services:** `docker compose up -d`
- **Stop Services:** `docker compose down`
- **View Logs:** `docker compose logs -f`
- **Restart Services:** `docker compose restart`
- **Stop and Remove Volumes:** `docker compose down -v --remove-orphans`

## Data Management

To reset the database and all associated Docker volumes, run the reset script:

```bash
./reset.sh
```

**Warning:** This command will permanently delete all data, including your `.env` file. Back up your `.env` file if you need to preserve your configuration.

## Development Environment (`dev/docker-compose.dev.yml`)

The `dev` environment is designed for local development and testing.

### Features

- **Local Studio Build:** The `studio` service is built from local source with a development Dockerfile, exposing the UI on port `8082`.
- **Mail Catcher:** An `inbucket` mail server is included to intercept and inspect outgoing emails. Access it at `http://localhost:9000`.
- **Test SMTP:** The `auth` service is pre-configured for a test SMTP server.
- **Ephemeral Database:** The `db` service is reset on every restart and automatically seeded with data from `dev/data.sql`.
- **Temporary Storage:** The `storage` service uses a temporary volume, ideal for testing.

### When to Use It

- When developing, testing, or experimenting with new features without affecting production data.
- When you need to inspect outgoing emails or quickly seed the database with sample data.

### How to Run

```bash
docker compose -f docker-compose.yml -f ./dev/docker-compose.dev.yml up -d
```

**Note:** This environment is not suitable for production use.

## Configuration

Key environment variables to configure in your `.env` file:

- `POSTGRES_PASSWORD`, `POSTGRES_DB`, `POSTGRES_HOST`, `POSTGRES_PORT`
- `JWT_SECRET`, `ANON_KEY`, `SERVICE_ROLE_KEY`
- `SUPABASE_PUBLIC_URL`, `SITE_URL`
- `SMTP_*` variables for email configuration.

## Troubleshooting

- **Port Conflicts:** Ensure required ports (5432, 3000, 8000) are not in use by other applications.
- **Startup Errors:** Check service logs (`docker compose logs <service_name>`) for configuration or environment variable issues.
- **Outdated Docker:** Make sure Docker and Docker Compose are updated to the required versions.
- **Volume Errors:** If you encounter volume-related issues, try removing the `volumes/` directory and restarting.

## References

- [Supabase Documentation](https://supabase.com/docs)
- [Supabase Docker Guide](https://supabase.com/docs/guides/hosting/docker)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Reference](https://docs.docker.com/compose/)

## Contributing

Contributions are welcome! To contribute:
- Fork the repository
- Create a new branch for your feature or bugfix
- Submit a pull request with a clear description
- For questions or suggestions, open an issue on GitHub

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

---
This setup is highly customizable. Feel free to modify the services and configurations to fit your project's needs.