# image build:
docker build -t <image name> -f <Dockerfile name> <Dockerfile path>

# run container:
docker run -d --name <container name> -p <port host:port container> <image name>

# for enter to container
docker exec -it <container name> /bin/bash/

# Bind volume folder to container
docker run -d -v <host path>:<container path> <image name>
