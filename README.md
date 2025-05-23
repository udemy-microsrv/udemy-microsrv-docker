﻿# udemy-microsrv-docker

Docker Compose to build and run the udemy-microsrv project, including the API Gateway, Microservices, databases, etc.

## Starting the application

1. To restore the project's submodules, run the following command in the project's root directory:
   ```bash
   git submodule update --init --recursive
   ```
2. Copy the content of the `.env.example` file to a file named `.env` in the project's root directory. Then, edit the `.env` file to configure the necessary environment variables for the application.
3. To run the application containers using Docker Compose, execute the following command in the project's root directory:
   ```bash
   docker compose up -d
   ```

## Adding a new submodule

1. Clone the repository of the submodule you want to add as a submodule in the project's directory:
   ```bash
   git submodule add <URL of the repository> <path to the submodule>
   git commit -m "feat: add submodule <path to the submodule>"
   git push origin
   ```

## Updating an existing submodule

1. Run the following commands to update the submodule:
   ```bash
   git submodule update --remote <path to the submodule>
   git commit -m "feat: update submodule <path to the submodule>"
   git push origin
   ```
