# Supported tags and respective `Dockerfile` links

-	[`3.3.7-jessie`, `3.3.7`, `3.3` (*3.3/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/fa7f071a42f11384d397a4b8533932f5660e5e71/3.3/Dockerfile)
-	[`3.6.6-jessie`, `3.6.6`, `3.6` (*3.6/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/fa7f071a42f11384d397a4b8533932f5660e5e71/3.6/Dockerfile)
-	[`3.9.10-jessie`, `3.9.10`, `3.9` (*3.9/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/5ac8a82d98c71eda8a718ad903d7e3ef15bd95ee/3.9/Dockerfile)
-	[`3.12.13-jessie`, `3.12.13`, `3.12`, (*3.12/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/1005674ff29c181d2ac2e8d05def922ff1542496/3.12/Dockerfile)
-   [`3.15.8-jessie`, `3.15.8`, `3.15` (*3.15/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/7768895552d0f1cb56de96fafedf28b357310d4b/3.15/Dockerfile)
-   [`3.18.2-jessie`, `3.18.2`, `3.18` (*3.18/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/1788e2f744aaa38d41261be978066cb2bfee6cce/3.18/Dockerfile)
-   [`3.19.1-jessie`, `3.19.1`, `3.19`, `3`, `latest` (*master/Dockerfile*)](https://github.com/baptistedonaux/docker-hhvm/blob/6c4ae926f68b40be87f559005a410e3007daf933/master/Dockerfile)

For more information about this image and its history, please see [the relevant manifest file (`baptistedonaux/hhvm`)](https://github.com/baptistedonaux/official-images/blob/master/library/hhvm). This image is updated via pull requests to [the `baptistedonaux/official-images` GitHub repo](https://github.com/baptistedonaux/official-images).

For detailed information about the virtual/transfer sizes and individual layers of each of the above supported tags, please see [the `hhvm/tag-details.md` file](https://github.com/baptistedonaux/docker-hhvm-doc/blob/master/hhvm/tag-details.md) in [the `baptistedonaux/docker-hhvm-doc` GitHub repo](https://github.com/baptistedonaux/docker-hhvm-doc).

# What is HHVM?

HHVM is an open-source virtual machine designed for executing programs written in Hack and PHP. HHVM uses a just-in-time (JIT) compilation approach to achieve superior performance while maintaining the development flexibility that PHP provides.

> [wikipedia.org/wiki/HipHop_Virtual_Machine](https://en.wikipedia.org/wiki/HipHop_Virtual_Machine)

![logo](https://raw.githubusercontent.com/baptistedonaux/docker-hhvm/master/logo.png)

# How to use this image

Two methods (CLI vs FastCGI):

## CLI mode

```console
$ docker run -v /foo.php:/foo.php hhvm foo.php
$ docker run -v /foo.php:/foo.php hhvm hhvm foo.php
$ docker run -v /foo.php:/foo.php hhvm php foo.php
```

## FastCGI mode

```console
$ docker run --name my_container_hhvm hhvm
```

### Complex configuration

Alternatively, a simple `Dockerfile` can be used to generate a new image that includes the necessary content (which is a much cleaner solution than the bind mount above):

```dockerfile
FROM hhvm:latest
RUN …

CMD ["hhvm", "-m", "server", "-vServer.Type=fastcgi", "-vServer.Port=9000", "--debug-sandbox=default"]
```

### How exposing port

```console
$ docker run --name hhvm -d -p 9000:9000 hhvm
```

### If I want use HHVM with Nginx container

Nginx configuration (default.conf)

```nginx
server {
    listen 80;
    root   /home/docker;

    location ~ \.php$ {
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass hhvm:9000;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
    }
}
```

#### From scratch

```console
$ docker run --name my_hhvm_container -v /path/to/project:/home/docker:rw -d hhvm:latest
$ docker run --name my_nginx_container -v /path/to/default.conf:/etc/nginx/conf.d/default.conf:ro -v /path/to/project:/home/docker:ro --link my_hhvm_container:hhvm -d nginx:latest
```

#### ... via [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `hhvm`:

```yaml
version: '2'

services:
	nginx:
	    image: nginx:latest
	    volumes:
	        - "/path/to/nginx.conf:/etc/nginx/nginx.conf:ro"
	        - "/path/to/project:/home/docker:ro"

	hhvm:
	    image: hhvm:latest
	    volumes:
	        - "/path/to/project:/home/docker:rw"
	    dns_search:
	        - "hhvm"
```

# License

This image is licensed under the MIT License (see [LICENSE](https://github.com/baptistedonaux/docker-hhvm/blob/master/LICENSE)).

# Supported Docker versions

This image supported on Docker version 1.10.0.

# User Feedback

## Documentation

Documentation for this image is stored in the [`hhvm/` directory](https://github.com/baptistedonaux/docker-hhvm-doc/tree/master/hhvm) of the [`baptistedonaux/docker-hhvm-doc` GitHub repo](https://github.com/baptistedonaux/docker-hhvm-doc). Be sure to familiarize yourself with the [repository's `README.md` file](https://github.com/baptistedonaux/docker-hhvm-doc/blob/master/hhvm/README.md) before attempting a pull request.

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/baptistedonaux/docker-hhvm/issues).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/baptistedonaux/docker-hhvm/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.
