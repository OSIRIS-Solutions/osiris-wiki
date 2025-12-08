---
tags:
  - Docker
  - container
  - installation
---

## Install as Docker container

> Many thanks to **Paul C. Gaida** for creating a Docker environment for OSIRIS.
 
## Prerequisites

To install OSIRIS in a Docker container, you need Docker on the host system. In addition, the OSIRIS source codes must be located in the current directory. Here you should also adjust the `CONFIG.php` file as described below in Basic settings](../configure/index.md).
 


## Create and start Docker container

To install OSIRIS in a Docker container, you must create the Docker environment with the following command:
 


**For a development environment:**

```bash
docker-compose -f docker-compose.dev.yml up -d
```

You can then access OSIRIS in the browser at `http://localhost:8080` (or at the address of the server on which Docker is running).
 

**For a production environment**

```bash
docker-compose -f docker-compose.prod.yml up -d
```

