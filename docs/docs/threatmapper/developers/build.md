---
title: Build from Source
---

# Build Deepfence ThreatMapper from Source

## Prerequisites

Build host:
 * 16 Gb RAM, 4 cores to build and run the Deepfence Management Console
 * Packages: build tools, `golang`, `docker`, `docker-compose`
   * Ubuntu/Debian: `apt install build-essential golang-go docker.io docker-compose`

If necessary, enable docker for the user account that will build the Deepfence containers:

```bash
sudo usermod -aG docker $USER # start new shell, or 'su $USER' for group change to take effect
```

## Building the Container Images

```bash
git clone https://github.com/deepfence/ThreatMapper.git
cd ThreatMapper
./build.sh
```

The build process will create a number of container images and store these in your local Docker repository.

## Running the Deepfence Management Console on the local machine

```bash
cd ThreatMapper/deployment-scripts
docker-compose -f docker-compose.yml up --detach
```

Once started, you can point a web browser at `https://--IP-ADDRESS---/` to register a first user on the Deepfence Management Console.  See [Initial Configuration](../console/initial-configuration) for more information.

To stop the Deepfence Management Console:

```bash
docker-compose -f docker-compose.yml down
```

## Push the images to a remote repository

If you plan to deploy the Management Console or Sensors (`deepfence_agent_ce` and `deepfence_discovery_ce`) on another host, you should push the images to a suitable, accessible repository:

For example, to push the images to DockerHub:

```bash
ACC=myorg             # the name of the dockerhub account 
docker login -u $ACC  # log in to the account
    
for IMG in \
    deepfence_agent_ce \
    deepfence_api_ce \
    deepfence_diagnosis_ce \
    deepfence_discovery_ce \
    deepfence_elastic_ce \
    deepfence_fetcher_ce \
    deepfence_init_ce \
    deepfence_postgres_ce \
    deepfence_redis_ce \
    deepfence_router_ce \
    deepfence_ui_ce \
    deepfence_vulnerability_mapper_ce
do
    docker tag deepfenceio/$IMG $ACC/$IMG:latest
    docker push $ACC/$IMG:latest
done
```
