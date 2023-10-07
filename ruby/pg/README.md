# 🐳 Docker Image for Ruby Development

This `Dockerfile` describes a base environment for Ruby development with PG and
Redis clients. It is based on [the official ruby
image](https://hub.docker.com/_/ruby).

The resulting image is pushed to dockerhub
`albandiguer/ruby-dev:tag+latest`

## Usage

To describe a ruby backend workload in a `compose.yml` use this image
`albandiguer/ruby-dev:latest`. 

```
x-common: &common
  image: albandiguer/ruby-dev:0.1
```



### Arguments

You can pass in a ` --build-arg ` in the build step to ajust following

| Argument | Default | Description |
| -------- | ------- | ----------- |
| `RUBY_VERSION ` | `3.2` | Ruby version |
| `DISTRO ` | `bookworm` | Debian distro |
| `PG_MAJOR ` | `15` | Postgres major version |


## Publishing

GH Actions publishes new version of this image on merge to master.

Ssee `docker-ruby-dev.ci.yml` in `.actions/` for dets.


### Manually 

List platforms available with
```
docker buildx ls 
```

Build (adjust tag in version.txt)
```
make build
```

Publish
```
make publish
```


