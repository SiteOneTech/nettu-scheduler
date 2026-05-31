FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

# Update default packages
RUN apt-get update \
    && apt-get install -y build-essential curl pkg-config libssl-dev \
    && apt-get -y dist-upgrade \
    && rm -rf /var/lib/apt/lists/*

ENV PATH="/root/.cargo/bin:${PATH}"

WORKDIR /home/rust/

COPY . .

ARG DATABASE_URL="postgresql://nettu:nettu_dev_password@127.0.0.1:55432/nettuscheduler"
ENV DATABASE_URL=${DATABASE_URL}

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
    && cargo build --release \
    && cp /home/rust/target/release/nettu_scheduler . \
    && cp /home/rust/target/release/migrate . \
    && rm -rf /home/rust/target \
    && rustup self uninstall -y

ENTRYPOINT ["/bin/sh", "-c", "if [ \"${MIGRATE_ON_START:-true}\" = \"true\" ]; then ./migrate; fi; ./nettu_scheduler"]
