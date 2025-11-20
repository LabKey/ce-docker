# LabKey ce-docker repository
LabKey Community Edition docker quick start guide.  This repo contains a generic docker-compose.yml file to aid in getting started with LabKey Community Edition. 



## **_Disclaimer_**

Note: integrations with LabKey products that traditionally have relied on OS configuration such as R reports or Python scripts will **NOT** work. 
But don't let that discourage you, as the rest of the community edition features are fully functional. If you feel you need those features, there are other options for installation.


## Requirements 

- Docker Engine or Docker Desktop

If you need help installing Docker, see these links for instructions:

- Install Docker on [Mac](https://docs.docker.com/desktop/setup/install/mac-install/)
- Install Docker on [Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
- Install Docker on [Linux](https://docs.docker.com/desktop/setup/install/linux/)


## Quick start
Once you have Docker installed, follow these steps to get started with LabKey Community Edition:

1. Open a command line window and clone this repo locally
`git clone https://github.com/LabKey/ce-docker.git`

1. CD to the cloned repo directory
`cd ./ce-docker`

1. Start LabKey Community Edition using docker compose
`docker compose up community --detach`

1. After a few minutes, LabKey Community Edition will have started, and you can log in to the application by opening a web browser and navigating to `https://localhost:8443`
2. You may see a warning about the connection being insecure, and need to click through 'advanced' or 'continue' to get to the setup wizard.
3. Using your web browser, complete the initial setup wizard.
4. Visit our docs page to learn more about how to [get started using LabKey](https://www.labkey.org/Documentation/wiki-page.view?name=gettingStarted).
 
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

### Upgrading versions
We only publish tagged versions to Docker Hub (we don't publish a 'latest' tag). To upgrade to a new version of LabKey Community edition, you have two options:
1. Edit the `docker-compose.yml` file and update the `image` version to the LabKey version you wish to use. 
2. Launch a new version with the `docker compose up` command line.
`export IDENT="labkeyteamcity/labkey-community:25.11.0" docker compose up community --detach`