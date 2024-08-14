# Build command:

```shell
docker build -t xavetar/mediawiki2latex:latest - < Dockerfile
```

## OR

```shell
docker build -t xavetar/mediawiki2latex:latest -f Dockerfile .
```

# Start command:

```shell
docker run -d --rm -p 8080:8080 xavetar/mediawiki2latex:latest
```
