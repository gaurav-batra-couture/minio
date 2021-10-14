# Local Setup Guide for Minio (tags/RELEASE.2021-06-14T01-29-23Z or before)

### Test the UI locally
___
- Follow the following instructions to setup the UI part of the project.

```bash
cd browser/
npm install
npm run dev
```
- If you want to generate a release just replace last command with `npm run release`.
- If you encounter the following error:

```bash
Error: No version of chokidar is available. Tried chokidar@2 and chokidar@3. after upgrading npm to 7.*.*
```

Use the following link: https://stackoverflow.com/questions/67042441/error-no-version-of-chokidar-is-available-tried-chokidar2-and-chokidar3-aft

### Build the minio binary
____

- In order to build the minio-binary make the project using following command in the parent directory:

```bash
make
```

- In order to verify the build run:

```bash
make verify
```

### Build the image
___
- In order to build an image first we need to create `minio.sha256sum` and `minio.minsign`. We use the following commands:

```bash
sha256sum minio > minio.sha256sum
```

```bash
b2sum -l 512 minio | awk '{printf $1}' | curl -f -F "digest=<-" https://minisig.me/sign -o minio.minisig
```

- Now, in order to build the image login to your docker account using `docker login` and use the following command:

```bash
docker buildx build --push --no-cache --build-arg RELEASE=RELEASE.2021-06-14T01-29-23Z -t "gauravbatra1998/minio:latest"     --platform=linux/arm64,linux/amd64,linux/ppc64le,linux/s390x -f Dockerfile.release .
```

Note - Image uses buildx to build the image, which can be installed from here: https://docs.docker.com/buildx/working-with-buildx/
