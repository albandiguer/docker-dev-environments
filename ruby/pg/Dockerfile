# syntax=docker/dockerfile:1.4

ARG RUBY_VERSION=3.2
ARG DISTRO=bookworm

FROM ruby:${RUBY_VERSION}-slim-${DISTRO}
ARG PG_MAJOR=15
ENV APP_HOME /usr/local/apps/app
RUN <<-EOF
	set -xe
	apt-get update
	apt-get install -qq -y --fix-missing --no-install-recommends \
		build-essential \
		ca-certificates \
		git \
		gnupg gnupg2 gnupg1 \
		libssl-dev \
		lsb-release \
		wget \
		libc6
	# Upgrade bundler
	gem update --system 3.4.20
	# PG file repository configuration
	sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" >\
		/etc/apt/sources.list.d/pgdg.list'
	# Import the repository signing key
	wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc |\
		tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
	apt-get update --allow-unauthenticated
	# Add pg & redis clients
	apt-get install -qq -y --fix-missing --no-install-recommends \
		postgresql-client-${PG_MAJOR} \
		postgresql-client-common \
		libpq-dev \
		redis-tools
	apt-get clean
	rm -rf /var/lib/apt/lists/*
EOF

COPY --chmod=777 <<-EOF /usr/local/bin/entrypoint.sh
	#!/bin/bash
	set -xe
	# Dev sugar
	echo 'alias db="psql \$DATABASE_URL"' >> ~/.bashrc
	# Run setup project db, clean /tmp dir (including pid) etc
	./bin/setup || true
	# Then exec the container's main process (what's set as CMD in the Dockerfile).
	exec "\$@"
EOF

ENTRYPOINT ["/usr/local/bin/entrypoint.sh" ]
WORKDIR ${APP_HOME}

CMD ["foreman", "start"]
