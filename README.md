[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]



<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/limitless-man/opencti">
    <img src="images/logo.png">
  </a>

<h3 align="center">OpenCTI Distributed Threat Intel Hub</h3>

  <p align="center">
    This GitHub project provides a distributed stack configuration for OpenCTI, a powerful open-source platform for cyber threat intelligence management. The stack is designed to enhance performance, scalability, and fault tolerance by utilizing various components.

Key Features:

Distributed MinIO: The stack incorporates MinIO, a distributed object storage system, which ensures high availability and fault tolerance for storing and managing large amounts of data. Load balancing is achieved through Nginx, further optimizing performance and resilience.

RabbitMQ Load Balancing: RabbitMQ, a message broker, is deployed in a distributed manner with load balancing to handle message queuing and ensure reliable communication between different components of the system. Nginx acts as a proxy to balance the traffic effectively.

Redis Clustering: Redis, an in-memory data structure store, is configured in a clustered setup, enabling fast and efficient data storage and retrieval. The cluster setup enhances performance and provides fault tolerance for handling high workloads.

Ingest-Analyst Separation: OpenCTI's functionality is split between the ingest and analyst components. The ingest module is responsible for connectors and data ingestion, while the analyst module focuses on the user interface for efficient analysis and visualization. This separation optimizes performance and enables better scaling for each component.

By leveraging these distributed components and load balancing techniques, this OpenCTI configuration aims to provide improved performance, fault tolerance, and scalability for handling large-scale cyber threat intelligence operations. The documented configuration on GitHub, along with instructions on using Portainer for deployment, facilitates easy setup and management of the entire stack.
    <br />
    <a href="https://github.com/limitless-man/opencti"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/limitless-man/opencti">View Demo</a>
    ·
    <a href="https://github.com/limitless-man/opencti/issues">Report Bug</a>
    ·
    <a href="https://github.com/limitless-man/opencti/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

[![Product Name Screen Shot][product-screenshot]](https://www.filigran.io/en/solutions/products/opencti/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

<!-- *
* [![Yaml][yaml.org]][Yaml-url] -->

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started with OpenCTI

These instructions will guide you through setting up OpenCTI locally. Follow these steps to get started.

### Prerequisites

Before getting started, make sure you have the following prerequisites installed:

- Docker
- Elasticsearch
- Kibana

## OpenCTI Setup Steps

### 1. Create an OpenCTI User

- Log in to your Elasticsearch cluster management UI.
- Navigate to the Security section or User Management section.
- Look for an option to create a new user and select it.
- Enter the following details:
  - Username: opencti
  - Password: [Enter a strong password for the OpenCTI user. Avoid using special characters, and ensure it meets any password complexity requirements.]
  - Full Name: OpenCTI User
- Save or Create the user to create the OpenCTI user with the provided details.

### 2. Create an OpenCTI Write Role

- In the Elasticsearch UI, navigate to the Security or Role Management section.
- Look for an option to create a new role and select it.
- Provide a name for the role, such as opencti_write.
- Configure the following privileges:
  - Indices:
    - Names: opencti* (to apply to all indices)
    - Index Privileges: All, Create, Delete, Read, Write
  - Cluster: Monitor, Manage_ilm, Manage_index_templates, create_snapshot, Cancel_task
- Save or Create the role to define the opencti_write role with the specified privileges.

### 3. Assign the OpenCTI Write Role to the OpenCTI User

- Go back to the User Management section or a similar section in the Elasticsearch UI.
- Find the OpenCTI user (opencti) in the list of users.
- Look for an option to edit or modify the user and select it.
- Locate the Roles or Role Membership section.
- Add the opencti_write role to the user's roles.
- Save or Apply the changes to assign the opencti_write role to the OpenCTI user.

### Additional Steps for docker swarm
   ```sh
   sudo docker network create -d overlay --attachable redis
   sudo docker network create -d overlay --attachable redis_proxy
   sudo docker network create -d overlay --attachable minio
   sudo docker network create -d overlay --attachable minio_proxy
   sudo docker network create -d overlay --attachable rabbitmq
   sudo docker network create -d overlay --attachable rabbitmq_proxy
   sudo docker network create -d overlay --attachable opencti_ingest
   sudo docker network create -d overlay --attachable opencti_ingest_proxy
   sudo docker network create -d overlay --attachable opencti_analyst
   sudo docker network create -d overlay --attachable opencti_analyst_proxy
   sudo docker network create -d overlay --attachable workers_int
   sudo docker network create -d overlay --attachable workers_ext
   sudo docker network create -d overlay --attachable internal-connectors
   sudo docker network create -d overlay --attachable external-connectors
   sudo docker network create -d overlay --attachable proxy
   sudo docker network create -d overlay --attachable proxy_platform_overlay
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/limitless-man/opencti.svg?style=for-the-badge
[contributors-url]: https://github.com/limitless-man/opencti/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/limitless-man/opencti.svg?style=for-the-badge
[forks-url]: https://github.com/limitless-man/opencti/network/members
[stars-shield]: https://img.shields.io/github/stars/limitless-man/opencti.svg?style=for-the-badge
[stars-url]: https://github.com/limitless-man/opencti/stargazers
[issues-shield]: https://img.shields.io/github/issues/limitless-man/opencti.svg?style=for-the-badge
[issues-url]: https://github.com/limitless-man/opencti/issues
[license-shield]: https://img.shields.io/github/license/limitless-man/opencti.svg?style=for-the-badge
[license-url]: https://github.com/limitless-man/opencti/blob/main/LICENSE
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/company/limitless-man/
[product-screenshot]: images/screenshot.png
[Yaml.org]: https://img.shields.io/badge/-YAML-000000?logo=YAML&logoColor=white&style=for-the-badge
[Yaml-url]: https://yaml.org
