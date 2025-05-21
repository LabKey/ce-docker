# LabKey ce-docker repository
LabKey Community Edition docker quick start guide.  This repo contains a generic docker-compose.yml file aid in getting started LabKey Community Edition. 



## **_Disclaimer_**

Note: integrations with LabKey products that traditionally have relied on OS configuration such as R reports or Python scripts will **NOT** work. 
But don't let that discourage you, as the rest of the community edition features are fully functional. 

## Requirements 

- Docker Engine or Docker Desktop

If you need help installing Docker, see this links for instructions:

- Install Docker on [Mac](https://docs.docker.com/desktop/setup/install/mac-install/)
- Install Docker on [Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
- Install Docker on [Linux](https://docs.docker.com/desktop/setup/install/linux/)


## Quick start
Once you have Docker installed, getting started is as easy as these steps:

1. Open and command line window and clone this repo locally
`git clone https://github.com/LabKey/ce-docker.git`

1. Start LabKey Community Edition using docker compose
`docker compose up community --detach`

1. After a few minutes, LabKey Community Edition will have started, and you can log in to the application by opening a web browser and navigating to `https://localhost:8443`
1. Using your web browser, complete the initial setup wizard. 
1. To learn more about how to [get started using LabKey](https://www.labkey.org/Documentation/wiki-page.view?name=gettingStarted)
 
## Stopping and clean up

1. To stop the instance
`docker compose down`
1. To stop the instance and discard/clean up
`docker-compose down -v --remove-orphans`
1. Some files are persisted locally and are not cleaned up.  Deleting any of these may result in data loss. Be careful!

- `mount/files` contains files saved in LabKey
- `mount/logs` contains LabKey Server logs
- `mount/pgdata` contains the postgresql database files
- `mount/modules` file path used for custom modules
 
## Advanced Configuration

There are a substantial number of configuration options available.  The `docker-compose.yml` included in this repo is designed to help most clients get started. 
[Additional configuration options are described here](https://github.com/LabKey/Dockerfile?tab=readme-ov-file#labkey). 