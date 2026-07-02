# Day 30 – Docker Images & Container Lifecycle

## Task
Today's goal is to **understand how images and containers actually work**.

You will:
- Learn the relationship between images and containers
- Understand image layers and caching
- Master the full container lifecycle

---

## Expected Output
- A markdown file: `day-30-images.md`
- Screenshots of key commands

---

## Challenge Tasks

### Task 1: Docker Images
1. Pull the `nginx`, `ubuntu`, and `alpine` images from Docker Hub
afrinz@Zs-MacBook-Air ~ % docker pull nginx                     
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:ec4ed8b5299e5e90694af7750eb6dffd2627317d30544d056b0371f8082f7bce
Status: Image is up to date for nginx:latest
docker.io/library/nginx:latest

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview nginx

similarly - docker pull alpine; docker pull ubuntu 

2. List all images on your machine — note the sizes
afrinz@Zs-MacBook-Air ~ % docker images
                                                                                                                                                                                            i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
alpine:latest        28bd5fe8b56d       13.6MB         4.27MB        
hello-world:latest   96498ffd522e       22.6kB         10.3kB    U   
nginx:latest         ec4ed8b5299e        259MB         64.3MB    U   
ubuntu:latest        53958ec7b67c        180MB         44.4MB       

3. Compare `ubuntu` vs `alpine` — why is one much smaller?
| Ubuntu                                         | Alpine                             |
| ---------------------------------------------- | ---------------------------------- |
| General-purpose Linux distribution             | Lightweight Linux distribution     |
| Includes many built-in utilities and libraries | Includes only essential components |
| Larger download size                           | Very small download size           |
| Easier for beginners                           | Popular for lightweight containers |
| Uses `apt` package manager                     | Uses `apk` package manager         |

4. Inspect an image — what information can you see?
afrinz@Zs-MacBook-Air ~ % docker inspect nginx
[
    {
        "Id": "sha256:ec4ed8b5299e5e90694af7750eb6dffd2627317d30544d056b0371f8082f7bce",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:ec4ed8b5299e5e90694af7750eb6dffd2627317d30544d056b0371f8082f7bce"
        ],
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2026-06-24T01:22:24.593848467Z",
        "Config": {
            "ExposedPorts": {
                "80/tcp": {}
            },

5. Remove an image you no longer need
afrinz@Zs-MacBook-Air ~ % docker rmi alpine
Untagged: alpine:latest
Deleted: sha256:28bd5fe8b56d1bd048e5babf5b10710ebe0bae67db86916198a6eec434943f8b

---

### Task 2: Image Layers
1. Run `docker image history nginx` — what do you see?
afrinz@Zs-MacBook-Air ~ % docker image history nginx
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
ec4ed8b5299e   3 days ago   CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      3 days ago   STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      3 days ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      3 days ago   COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
<missing>      3 days ago   COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
<missing>      3 days ago   COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
<missing>      3 days ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
<missing>      3 days ago   COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
<missing>      3 days ago   RUN /bin/sh -c set -x     && groupadd --syst…   84.9MB    buildkit.dockerfile.v0
<missing>      3 days ago   ENV DYNPKG_RELEASE=1~trixie                     0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENV PKG_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENV ACME_VERSION=0.4.1                          0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENV NJS_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENV NJS_VERSION=0.9.9                           0B        buildkit.dockerfile.v0
<missing>      3 days ago   ENV NGINX_VERSION=1.31.2                        0B        buildkit.dockerfile.v0
<missing>      3 days ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      4 days ago   # debian.sh --arch 'arm64' out/ 'trixie' '@1…   109MB     debuerreotype 0.17
Each line is a **layer**. Note how some layers show sizes and some show 0B

2. Write in your notes: What are layers and why does Docker use them?

---

### Task 3: Container Lifecycle
Practice the full lifecycle on one container:
1. **Create** a container (without starting it)
afrinz@Zs-MacBook-Air ~ % docker create --name my-nginx nginx
76ef7004d666cc8bada4dfff220ad55c768d1c539089c3d9130a6a8e51728ec6

2. **Start** the container
afrinz@Zs-MacBook-Air ~ % docker start my-nginx
my-nginx

3. **Pause** it and check status
afrinz@Zs-MacBook-Air ~ % docker pause my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                   PORTS     NAMES
76ef7004d666   nginx     "/docker-entrypoint.…"   55 seconds ago   Up 43 seconds (Paused)   80/tcp    my-nginx

4. **Unpause** it
afrinz@Zs-MacBook-Air ~ % docker unpause my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps              
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
76ef7004d666   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    my-nginx

5. **Stop** it
afrinz@Zs-MacBook-Air ~ % docker stop my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

6. **Restart** it
afrinz@Zs-MacBook-Air ~ % docker restart my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
76ef7004d666   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 seconds   80/tcp    my-nginx

7. **Kill** it
afrinz@Zs-MacBook-Air ~ % docker kill my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps           
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

8. **Remove** it
afrinz@Zs-MacBook-Air ~ % docker rm my-nginx
my-nginx
afrinz@Zs-MacBook-Air ~ % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

Check `docker ps -a` after each step — observe the state changes.

---

### Task 4: Working with Running Containers
1. Run an Nginx container in detached mode
docker run -d --name my-ubuntu ubuntu 

2. View its **logs**
afrinz@Zs-MacBook-Air ~ % docker logs my-ubuntu

3. View **real-time logs** (follow mode)
docker logs -f my-ubuntu

4. **Exec** into the container and look around the filesystem
docker exec -it my-ubuntu bash

5. Run a single command inside the container without entering it
docker exec my-nginx ls /

6. **Inspect** the container — find its IP address, port mappings, and mounts
afrinz@Zs-MacBook-Air ~ % docker inspect my-ubuntu
[
    {
        "Id": "7c8ffdd754f449fdd44e0665a58b879fc49344d39dfad4b6b9fd3321eddd89a3",
        "Created": "2026-06-27T08:16:05.556961042Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 127,
            "Error": "",
            "StartedAt": "2026-06-27T08:16:05.584954167Z",
            "FinishedAt": "2026-06-27T08:36:01.632979179Z"
        },


---

### Task 5: Cleanup
1. Stop all running containers in one command
docker stop $(docker ps -q)

2. Remove all stopped containers in one command
afrinz@Zs-MacBook-Air ~ % docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
7c8ffdd754f449fdd44e0665a58b879fc49344d39dfad4b6b9fd3321eddd89a3
a9ac89cf45cb81c0561a4b40ca75f3f1927d090723f2e6d48da9b30a40472698
d7717c7cca673b73ee360d6499c4587773f2873ce932d7639ab380628754bfde
fb6e2aeadf2ea84eb298d16a646b6d12d4719d527bce0261051e5998cfcca22f

Total reclaimed space: 32.77kB

3. Remove unused images
afrinz@Zs-MacBook-Air ~ % docker image prune
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Deleted Images:
untagged: sha256:f3d28607ddd78734bb7f71f117f3c6706c666b8b76cbff7c9ff6e5718d46ff64
deleted: sha256:f3d28607ddd78734bb7f71f117f3c6706c666b8b76cbff7c9ff6e5718d46ff64
deleted: sha256:98ce78d9714d15b1785dd97da22ec7c2506270ff7c910160bc07e393566f7568
deleted: sha256:98e9bec2e99ed8a81fec9634a00d7c1eea6fd8f8689e87dd7d3723383fc8a7e3

Total reclaimed space: 44.45MB

4. Check how much disk space Docker is using
afrinz@Zs-MacBook-Air ~ % docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          3         0         438.2MB   438.1MB (99%)
Containers      0         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B


---

## Hints
- Image history: `docker image history`
- Create without starting: `docker create`
- Follow logs: `docker logs -f`
- Inspect: `docker inspect`
- Cleanup: `docker system df`, `docker system prune`

---

