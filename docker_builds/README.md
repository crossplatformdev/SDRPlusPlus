# Docker Builds

This directory contains Docker build configurations for building SDR++ on various Linux distributions.

## Building a Specific Commit

All build scripts now support building from a specific commit of the SDRPlusPlus repository. This is controlled by the `SDRPP_REPO` and `SDRPP_COMMIT` environment variables.

### Default Behavior

By default, if the `SDRPlusPlus` directory is not present (not mounted from the host), the build scripts will:
- Clone from repository: `https://github.com/AlexandreRouma/SDRPlusPlus`
- Checkout commit: `052167962dbf9adc2a02825f2f428e7613255d50`

### Building with a Custom Commit or Repository

To build from a different repository or commit, set the environment variables when running the Docker container:

```bash
# Example: Building Ubuntu Jammy with a specific commit
docker run --name build \
  --env SDRPP_COMMIT=<commit-sha> \
  sdrpp_build /root/do_build.sh

# Example: Building from a fork repository
docker run --name build \
  --env SDRPP_REPO=https://github.com/youruser/SDRPlusPlus \
  --env SDRPP_COMMIT=<commit-sha> \
  sdrpp_build /root/do_build.sh
```

### Building with Mounted Source

If you want to build from a local checkout of SDRPlusPlus (e.g., for development), mount the directory as before:

```bash
# Example: Building with mounted source
docker run --name build \
  -v /path/to/SDRPlusPlus:/root/SDRPlusPlus \
  --env BUILD_NO="-$GITHUB_RUN_NUMBER" \
  sdrpp_build /root/do_build.sh
```

When a directory is mounted at `/root/SDRPlusPlus`, the build script will use it instead of cloning from GitHub.

## Available Distributions

- Debian Bookworm
- Debian Bullseye
- Debian Sid
- Debian Trixie
- Ubuntu Bionic (18.04)
- Ubuntu Focal (20.04)
- Ubuntu Jammy (22.04)
- Ubuntu Noble (24.04)
- Ubuntu Oracular (24.10)

## Examples

### Build latest code
```bash
cd docker_builds/ubuntu_jammy
docker build . --tag sdrpp_build
docker run --name build sdrpp_build /root/do_build.sh
docker cp build:/root/SDRPlusPlus/sdrpp_debian_amd64.deb ./
```

### Build specific commit
```bash
cd docker_builds/ubuntu_jammy
docker build . --tag sdrpp_build
docker run --name build --env SDRPP_COMMIT=abc123def456 sdrpp_build /root/do_build.sh
docker cp build:/root/SDRPlusPlus/sdrpp_debian_amd64.deb ./
```

### Build from local source
```bash
cd docker_builds/ubuntu_jammy
docker build . --tag sdrpp_build
docker run --name build -v $PWD/../..:/root/SDRPlusPlus sdrpp_build /root/do_build.sh
docker cp build:/root/SDRPlusPlus/sdrpp_debian_amd64.deb ./
```
