# Go (golang) Docker image with Alpine-glibc
This image based on [Go (golang)](https://hub.docker.com/_/golang) official image, and contains glibc to enable proprietary projects compiled against glibc work on Alpine.

This image includes some quirks to make [glibc](https://www.gnu.org/software/libc/) work side by side with musl libc (default in Alpine Linux). glibc packages for Alpine Linux are prepared by [Sasha Gerrand](https://github.com/sgerrand) and the releases are published in [sgerrand/alpine-pkg-glibc](https://github.com/sgerrand/alpine-pkg-glibc) github repo.

If you need to update your libc library cache, use `/usr/glibc-compat/sbin/ldconfig` instead of the usual `/sbin/ldconfig`. You can also use the `LD_LIBRARY_PATH` as on standard libc-based distributions.

## Supported tags
No `latest` tag, please use specific version tags.

 - `1.23-alpine-glibc`

## Docker Pull Command

```console
docker pull sirmark/golang:1.23-alpine-glibc
```

## Usage Example
Pull and run.
```console
$ docker pull sirmark/golang:1.23-alpine-glibc
$ docker run --rm -it sirmark/golang:1.23-alpine-glibc
```

Use in your `Dockerfile`, writing something along the lines of the following will compile and run your project:
```dockerfile
FROM sirmark/golang:1.23-alpine-glibc

WORKDIR /usr/src/app

# pre-copy/cache go.mod for pre-downloading dependencies and only redownloading them in subsequent builds if they change
COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN go build -v -o /usr/local/bin/app ./...

CMD ["app"]
```
Then, build and run the Docker image:

```console
$ docker build -t my-golang-app .
$ docker run -it --rm --name my-running-app my-golang-app
```
