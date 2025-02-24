# Slurm REST Docker Cluster with HyPhy

The idea currently is that this is useful to develop against. If we want to put this image into production, it probably means investigating Docker Swarm or something. But either way, this repo should prove very useful for anyone attempting local development of the backend of datamonkey3.

# Table of contents

- [Slurm REST Docker Cluster](#slurm-rest-docker-cluster)
- [Table of contents](#table-of-contents)
- [Introduction](#introduction)
- [REST API](#rest-api)

# Introduction

[This is a fork of the SLURM REST Docker repo here.](https://github.com/JBris/slurm-rest-api-docker)

[Which is in turn a fork of the SLURM Docker repo here.](https://github.com/giovtorres/slurm-docker-cluster)

[The principal difference is that the dependencies required for HyPhy have been included in the image build.](https://github.com/veg/hyphy)

# REST API

1. Run `docker compose up -d` to launch a local SLURM cluster.
2. Within the *c2* node, set the JSON web token: `export $(docker compose exec c2 scontrol token)`
3. Test that you can access the SLURM API documentation: `curl -k -vvvv -H X-SLURM-USER-TOKEN:${SLURM_JWT} -H X-SLURM-USER-NAME:root -X GET 'http://localhost:9200/openapi/v3' > docs.json`
4. Submit a SLURM job: `curl -X POST "http://localhost:9200/slurm/v0.0.37/job/submit" -H "X-SLURM-USER-NAME:root" -H "X-SLURM-USER-TOKEN:${SLURM_JWT}" -H "Content-Type: application/json" -d @rest_api_test.json`
5. Check that the SLURM job completed successfully: `docker compose exec c1 cat /root/test.out`
6. Submit a job to test HyPhy installation: `curl -X POST "http://localhost:9200/slurm/v0.0.37/job/submit" -H "X-SLURM-USER-NAME:root" -H "X-SLURM-USER-TOKEN:${SLURM_JWT}" -H "Content-Type: application/json" -d @hyphy_test.json`
7. Check the HyPhy version reported successfully: `docker compose exec c1 cat /root/hyphy_test.out`
