# image build:
docker build -t apache_cmd -f <name> <Dockerfile path>

# run container:
docker run -d --name <container name> -p <port host:port container> <image name>
