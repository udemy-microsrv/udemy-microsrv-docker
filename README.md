# udemy-microsrv-docker

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

## Starting in production mode

To run the application in production mode using Docker Compose, use the following command:

```bash
docker compose -f docker-compose.prod.yml up --build
```

This command will build and start the application containers as defined in the `docker-compose.prod.yml` file. **Ensure that all required environment variables are set before starting.**

## Starting Kubernetes cluster

> Make sure you have [Helm](https://helm.sh/) installed and configured to use your Kubernetes cluster.

To deploy the Kubernetes cluster for this project, follow these steps:

1. Ensure you have a service account with the `roles/artifactregistry.reader` role and a key file (e.g., `k8s-ar-reader-key.json`). Create a Kubernetes secret for Docker registry authentication:
   ```bash
   kubectl create secret docker-registry gcp-artifact-registry-creds \
      --docker-server=https://us-central1-docker.pkg.dev \ # Replace with your AR region
      --docker-username=_json_key \
      --docker-password="$(cat ./k8s-ar-reader-key.json)" \
      --docker-email=your-email@example.com # Can be any valid email
   ```
2. Patch the default service account to use this secret for pulling images:
   ```bash
   kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "gcp-artifact-registry-creds"}]}'
   ```
3. Create Kubernetes secrets for each microservice (check `deployment.yml` files in `./k8s/udemy-microsrv-cluster/templates` to see all required secrets):
   ```bash
   kubectl create secret generic udemy-microsrv-auth.secrets --from-literal=foo=bar
   kubectl create secret generic udemy-microsrv-order.secrets --from-literal=foo=bar
   kubectl create secret generic udemy-microsrv-payment.secrets --from-literal=foo=bar
   ```
4. To install the Helm chart for the project:
   ```bash
   helm install udemy-microsrv-cluster ./k8s/udemy-microsrv-cluster
   ```
   To upgrade the deployment after making changes:
   ```bash
   helm upgrade udemy-microsrv-cluster ./k8s/udemy-microsrv-cluster
   ```
