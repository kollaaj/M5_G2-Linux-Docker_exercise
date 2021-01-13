# Docker notes for Back-End Operating Systems

## Create a Dockerized Linux installation (via Dockerfile)

**Example of a Dockerfile:**
```
FROM node:12-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js]
```

**Build an image from Dockerfile:**

`docker build -t <name> .`

Note that you have to be in the same folder as the Dockerfile


**Example:**
`docker build -t watevs .`

**Start the container from the image you created:**

`docker run [<options>] <image>`

**Example:**
`docker run -d -p 80:3000 watevs`

## List a few commands that will show information about memory, processes, CPU and mounted devices, which should work on the Dockerized Linux

There are a few ways to run commands on a Docker container that I know of:

**1. Start the container in interactive mode:**

`docker run -it [<options>] <image> "/bin/sh"`

**Example:**
`docker run -it -p 80:3000 watevs "/bin/sh"`

or

`docker run -it --entrypoint "/bin/sh" watevs`

The thing with this command is that when you leave the interactive mode with `exit` the container shuts off.


**2. Start the container normally and then open interactive mode:**

```
docker run -d [<options>] <image>
docker exec -it <id> <command>
```

To find the ID run `docker ps`

**Example:**
```
docker run -d -p 80:3000 watevs
docker exec -it c9dd5fda0e79 cat /proc/meminfo
```

**3. Start the container normally and then run one command:**
```
docker run -d [<options>] <image>
docker exec -it [<options>] <id> <skipun>
```

**Example:**
```
docker run -d -p 80:3000 watevs
docker exec -it c9dd5fda0e79 cat /proc/meminfo
```

**Memory:**

`docker exec -it <id> cat /proc/meminfo`

or

`docker exec -it <id> free -m`

**Processes:**

`docker exec -it <id> ps aux`

**CPU:**

`docker exec -it <id> cat /proc/cpuinfo`

**Mounted devices:**

`docker exec -it <id> df -h`

**Kill a process:** 

This is done in two steps, so start by opening interactive mode (step 1 or 2 above).

First find the PID: 

`docker exec -it <id> ps aux`

Then kill the process:
`docker exec -it <id> kill -9 <PID>`

**Mount a folder (or hard drive/other-stuff) when the container is run:**

`docker run -d -v <path to a folder on the computer>:<path to a folder in the container> <image>`

**Example:** `docker run -d -p 80:3000 -v //c/path/to/app:/app watevs`

Note you have to replace `//c/path/to/app` out for the path to the project.

