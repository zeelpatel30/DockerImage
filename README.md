version: '3.0'

services:

    docker-registry:
        container_name: docker-registry
        restart: always
        image: registry:2
        ports:
            - 5000:5000
        environment:

            REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
            REGISTRY_HTTP_TLS_KEY: /certs/domain.key
        volumes:
            - docker-registry-data:/var/lib/registry
            - ./certs:/certs
    docker-registry-ui:
        container_name: docker-registry-ui
        image: konradkleine/docker-registry-frontend:v2
        ports:
            - 8080:80
        environment:
            ENV_DOCKER_REGISTRY_HOST: docker-registry
            ENV_DOCKER_REGISTRY_PORT: 5000
volumes:
  docker-registry-data: {}
