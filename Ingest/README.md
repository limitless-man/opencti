<!-- OpenCTI Ingest Component Description -->

## OpenCTI Ingest Component

The OpenCTI Ingest component plays a crucial role in the OpenCTI platform by handling background tasks related to data ingestion. It is responsible for retrieving data from various connectors and processing it to enrich the intelligence database. By separating the ingestion process from the analyst dashboard, the OpenCTI Ingest component ensures that the data ingestion tasks run smoothly without impacting the analyst's experience.

### Docker Compose Configuration

The `docker-compose.yml` file defines the following services for the OpenCTI Ingest component:

- **OpenCTI Ingest Instances**: The stack includes multiple instances of the OpenCTI Ingest application, represented by the services `opencti_ingest1`, `opencti_ingest2`, `opencti_ingest3`, and `opencti_ingest4`. These instances are based on the latest `opencti/platform` Docker image. By deploying multiple replicas of the OpenCTI Ingest, the component achieves high availability and load balancing, ensuring efficient data ingestion and processing.

- **Nginx Reverse Proxy**: The Nginx service acts as a reverse proxy for the OpenCTI Ingest instances. It utilizes the `nginx:1.19.2-alpine` Docker image and is responsible for routing incoming requests to the available OpenCTI Ingest replicas. The Nginx configuration is provided through the `proxy_ingest` config, which is mounted into the Nginx container.

The Nginx configuration (`proxy_ingest`) is designed to distribute incoming requests across the available OpenCTI Ingest replicas. The Nginx server listens on port 8080 and forwards the requests to the OpenCTI Ingest instances (`opencti_ingest1`, `opencti_ingest2`, `opencti_ingest3`, and `opencti_ingest4`). The load balancing algorithm is applied to distribute the workload evenly and ensure optimal resource utilization.

By deploying this stack, you can effectively run and scale multiple instances of the OpenCTI Ingest component, enabling efficient data ingestion and processing for your OpenCTI platform.

Please note that the provided configuration is a snippet, and you may need to customize it further based on your specific environment and requirements.
