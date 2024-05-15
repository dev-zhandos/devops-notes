# Docker Compose Cheat Sheet

Docker Compose simplifies the management of multi-container applications. It uses a YAML file (usually named `docker-compose.yml`) to define services, networks, and volumes.

## Table of Contents

- Commands
- Configuration (docker-compose.yml)
- Tips and Best Practices


## Commands

| Command                      | Description                                                                  | Example                                               |
| ----------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------- |
| `docker-compose up`          | Builds, (re)creates, starts, and attaches to containers for all services      | `docker-compose up -d` (detached mode)                 |
| `docker-compose down`        | Stops and removes containers, networks, volumes, and images created by `up`  | `docker-compose down`                                |
| `docker-compose start`       | Starts existing containers for a service                                       | `docker-compose start web`                            |
| `docker-compose stop`        | Stops running containers without removing them                               | `docker-compose stop db`                             |
| `docker-compose restart`     | Restarts a service's containers                                               | `docker-compose restart web`                          |
| `docker-compose build`       | Builds or rebuilds services                                                  | `docker-compose build --no-cache web`                 |
| `docker-compose ps`          | Lists containers                                                              | `docker-compose ps`                                   |
| `docker-compose logs`        | Displays logs for a service                                                   | `docker-compose logs -f web` (follow logs)             |
| `docker-compose exec`        | Executes a command in a running container                                    | `docker-compose exec web bash`                        |
| `docker-compose run`         | Runs a one-off command in a service's container                                | `docker-compose run web python manage.py migrate`     |


## Configuration (docker-compose.yml)

```yaml
version: '3.8'  # Specify Compose file version

services:
  web:       # Service name
    build: .   # Build from Dockerfile in current directory
    ports:     # Map ports
      - "8000:80"
    volumes:   # Mount volumes
      - .:/code  
    depends_on:  # Start dependencies before this service
      - db
  db:
    image: postgres  # Pull image from registry
    environment:   # Set environment variables
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:       # Mount volumes (for persistent data)
      - db_data:/var/lib/postgresql/data
volumes:   # Declare named volumes
  db_data: 
```

**Common Configuration Options:**

- **image:** Specify the Docker image to use.
- **build:** Specify the path to a Dockerfile or a build context.
- **command:** Override the default command to run in the container.
- **ports:** Map host ports to container ports.
- **volumes:** Mount host directories or named volumes into the container.
- **environment:** Set environment variables.
- **depends_on:** Control the startup order of services.
- **networks:** Create or connect to custom networks.
- **deploy:** Configure deployment options (replicas, resources, restart policy).


## Tips and Best Practices

- **Use a Version Control System:** Keep your `docker-compose.yml` file in a version control system (like Git) for tracking changes.
- **Environment Variables:** Store sensitive information (e.g., passwords) in environment variables.
- **Named Volumes:** Use named volumes for data persistence.
- **Multiple Compose Files:** Use multiple Compose files (`docker-compose.yml`, `docker-compose.override.yml`) for different environments (development, production).
- **Docker Compose Profiles:** Activate specific services based on profiles.
