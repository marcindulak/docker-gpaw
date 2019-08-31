# Description

Dockerfile for GPAW https://wiki.fysik.dtu.dk/gpaw/ built against openmpi, based on Fedora.


# Usage

First, make sure you are able to run the `docker run hello-world` example https://docs.docker.com/get-started/

Then test the basic GPAW functionality

```sh
docker run --rm -it marcindulak/gpaw-openmpi:latest bash -c '. /etc/profile.d/modules.sh&& module use /usr/share/modulefiles&& module load mpi/openmpi-x86_64&& mpiexec --allow-run-as-root -np 2 gpaw-python3_openmpi -c "import gpaw.mpi; print(gpaw.mpi.rank)"'
```

When running GPAW jobs inside of the container you want the output files created by GPAW to
be accessible locally on the machine running the job.
In order to achieve this create a local storage directory for this particular myjob.

```sh
mkdir myjob
```

Create a GPAW input script and save into the `myjob` directory.

Then start the job mounting the local storage directory as a docker volume https://docs.docker.com/storage/volumes/ inside of the container

## Run a job manually

```sh
docker run --name myjob --rm -it -v "$(pwd)/myjob:/mnt" marcindulak/gpaw-openmpi:latest bash -c '. /etc/profile.d/modules.sh&& module use /usr/share/modulefiles&& module load mpi/openmpi-x86_64&& cd /mnt&& mpiexec --allow-run-as-root -np 2 which gpaw-python3_openmpi h2.py'
```

## Run a job with docker-compose

Preferably run the job using `docker-compose`

```sh
docker-compose -f docker-compose.myjob.yml up
```

Remove the terminated container

```sh
docker-compose -f docker-compose.myjob.yml down
```

## Examine the created output file

```sh
cat myjob/h2.txt
```


# Building

## Locally

Build based on the local [Dockerfile](Dockerfile)

```sh
docker build -t gpaw-openmpi .
```

List images

```sh
docker images
```


# Dependencies

docker and optionally docker-compose


# License of this Dockerfile

Apache-2.0

**Note** please consult the GPAW software https://gitlab.com/gpaw/gpaw/blob/master/LICENSE


# Todo