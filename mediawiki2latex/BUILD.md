Build command:

docker build -t xavetar/mediawiki2latex:latest - < Dockerfile

Start command:

docker run --rm -p 8080:8080 --user "$(id -u)":"$(id -g)" xavetar/mediawiki2latex:latest
