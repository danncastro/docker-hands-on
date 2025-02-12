ARG RUST_VERSION=1.79
FROM docker.io/library/rust:${RUST_VERSION} AS build
WORKDIR /app
COPY . /app

RUN apt-get update && apt-get install -y \
    cmake \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN cargo install --path .
RUN strip /usr/local/cargo/bin/oha

# Target image
FROM registry.fedoraproject.org/fedora-minimal
USER 65535

COPY --chown=65535:65535 --from=build /usr/local/cargo/bin/oha /bin/oha

ENTRYPOINT ["/bin/oha"]

# docker build -t oha-load-testing .
# docker run --rm oha-load-testing -u https://www.exemplo.com -n 5000 -c 100 -t 60s

# -u https://www.exemplo.com: A URL do seu site ou aplicação que será testada.
# -n 1000: Número total de requisições a serem feitas.
# -c 50: Número de conexões simultâneas.
# -t 60: Tempo de execução