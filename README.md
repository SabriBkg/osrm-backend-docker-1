# osrm-backend-docker
[![](https://images.microbadger.com/badges/image/peterevans/osrm-backend.svg)](https://microbadger.com/images/peterevans/osrm-backend)
[![CircleCI](https://circleci.com/gh/peter-evans/osrm-backend-docker/tree/master.svg?style=svg)](https://circleci.com/gh/peter-evans/osrm-backend-docker/tree/master)

Docker image for the Open Source Routing Machine (OSRM) [osrm-backend](https://github.com/Project-OSRM/osrm-backend).

## Supported tags and respective `Dockerfile` links

- [`1.19.1`, `1.19`, `latest`  (*1.19/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master)
- [`1.19.1-trusty`, `1.19-trusty`, `trusty`  (*1.19/trusty/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/trusty)
- [`1.18.0`, `1.18` (*1.18/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.18)
- [`1.18.0-trusty`, `1.18-trusty` (*1.18/trusty/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.18/trusty)
- [`1.17.0`, `1.17` (*1.17/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.17)
- [`1.17.0-trusty`, `1.17-trusty` (*1.17/trusty/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.17/trusty)
- [`1.16.0`, `1.16` (*1.16/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.16)
- [`1.16.0-trusty`, `1.16-trusty` (*1.16/trusty/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.16/trusty)
- [`1.15.1`, `1.15` (*1.15/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.15)
- [`1.15.1-trusty`, `1.15-trusty` (*1.15/trusty/Dockerfile*)](https://github.com/peter-evans/osrm-backend-docker/tree/master/archive/1.15/trusty)

For earlier versions see [releases](https://github.com/peter-evans/osrm-backend-docker/releases) and the available [tags on Docker Hub](https://hub.docker.com/r/peterevans/osrm-backend/tags/).

## Usage
Pass the `OSRM_PBF_URL` environment variable to the container referencing the URL of your PBF file:

```bash
docker run -d -p 5000:5000 \
-e OSRM_PBF_URL='http://download.geofabrik.de/asia/maldives-latest.osm.pbf' \
--name osrm-backend peterevans/osrm-backend:latest
```
The PBF file will be downloaded and the graph will begin building. Note that very large graphs may take hours to be built.

Tail the logs to verify the graph has been built and osrm-backend is serving requests:
```
docker logs -f <CONTAINER ID>
```
Then point your web browser to [http://localhost:5000/](http://localhost:5000/)

For API documentation see [http://project-osrm.org/docs/v5.15.2/api/](http://project-osrm.org/docs/v5.15.2/api/)

## Graph Profiles
The graph profile will default to `car`. Other profiles can be specified with the `OSRM_GRAPH_PROFILE` environment variable:
```bash
docker run -d -p 5000:5000 \
-e OSRM_PBF_URL='http://download.geofabrik.de/asia/maldives-latest.osm.pbf' \
-e OSRM_GRAPH_PROFILE='bicycle' \
--name osrm-backend peterevans/osrm-backend:latest
```
Available profiles are `car`,`bicycle` and `foot`.

## Custom Graph Profiles
The URL to a custom graph profile can be passed via the `OSRM_GRAPH_PROFILE_URL` environment variable. If this variable is set it will override any profile set by `OSRM_GRAPH_PROFILE`.
```bash
docker run -d -p 5000:5000 \
-e OSRM_PBF_URL='http://download.geofabrik.de/asia/maldives-latest.osm.pbf' \
-e OSRM_GRAPH_PROFILE_URL='https://raw.githubusercontent.com/peter-evans/osrm-backend-docker/master/tests/car.lua' \
--name osrm-backend peterevans/osrm-backend:latest
```

## Data Storage Location
By default the graph will be built and stored at the path `/osrm-data`. A custom path can be specified with the `OSRM_DATA_PATH` environment variable. Note that the path should NOT contain a trailing slash (`/`).
```bash
docker run -d -p 5000:5000 \
-e OSRM_PBF_URL='http://download.geofabrik.de/asia/maldives-latest.osm.pbf' \
-e OSRM_DATA_PATH='/my-custom-path' \
--name osrm-backend peterevans/osrm-backend:latest
```

## Persistent Storage
For a solution to persisting graph data and immutable deployments check out [osrm-backend for Kubernetes](https://github.com/peter-evans/osrm-backend-k8s).

## License

MIT License - see the [LICENSE](LICENSE) file for details
