<!-- OpenCTI Analyst Component Description -->

## OpenCTI Analyst Component

The OpenCTI Analyst component serves as the frontend dashboard for analysts using the OpenCTI platform. It provides a user-friendly interface and powerful features to support efficient analysis and investigation of intelligence data. By separating the analyst dashboard from the background components, such as data ingestion and processing, the OpenCTI Analyst component ensures that analysts can perform their tasks without experiencing slowdowns or performance issues.

### Docker Compose Configuration

The `docker-compose.yml` file defines the following services for the OpenCTI Analyst component:

- **OpenCTI Analyst Instances**: The stack includes multiple instances of the OpenCTI Analyst application, represented by the services `opencti_analyst1`, `opencti_analyst2`, `opencti_analyst3`, and `opencti_analyst4`. These instances are based on the latest `opencti/platform` Docker image. By deploying multiple replicas of the OpenCTI Analyst, the component achieves high availability and load balancing.

- **Nginx Reverse Proxy**: The Nginx service acts as a reverse proxy for the OpenCTI Analyst instances. It utilizes the `nginx:1.19.2-alpine` Docker image and is responsible for routing incoming requests to the available OpenCTI Analyst replicas. The Nginx configuration is provided through the `proxy_analyst` config, which is mounted into the Nginx container.

The Nginx configuration (`proxy_analyst`) is designed to distribute incoming requests across the available OpenCTI Analyst replicas. It uses the `ip_hash` load balancing algorithm to ensure session persistence. The Nginx server listens on port 8080 and forwards the requests to the OpenCTI Analyst instances (`opencti_analyst1`, `opencti_analyst2`, `opencti_analyst3`, and `opencti_analyst4`).

By deploying this stack, you can effectively run and scale multiple instances of the OpenCTI Analyst component, allowing analysts and investigators to access the OpenCTI platform simultaneously while ensuring fault tolerance and optimal performance.

Please note that the provided configuration is a snippet, and you may need to customize it further based on your specific environment and requirements.