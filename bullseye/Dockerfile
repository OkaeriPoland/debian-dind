FROM debian:bullseye-slim


ARG DOCKER_CHANNEL=stable
ARG DOCKER_VERSION=5:20.10.12~3-0~debian-bullseye

RUN apt-get update && apt-get install -y --no-install-recommends apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) ${DOCKER_CHANNEL}"
RUN apt-get update && apt-get install -y --no-install-recommends docker-ce=${DOCKER_VERSION} && docker -v && dockerd -v


ARG DOCKER_COMPOSE_VERSION=v2.2.2
RUN curl -L https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-linux-x86_64 > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose


ARG DOCKER_BRANCH=20.10
ENV DOCKER_EXTRA_OPTS '--storage-driver=overlay'
RUN curl -fL -o /usr/local/bin/dind "https://raw.githubusercontent.com/moby/moby/${DOCKER_BRANCH}/hack/dind" && chmod +x /usr/local/bin/dind


COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

VOLUME /var/lib/docker
EXPOSE 2375
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
