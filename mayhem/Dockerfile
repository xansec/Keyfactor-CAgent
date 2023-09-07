FROM ubuntu AS builder

RUN apt update -y && DEBIAN_FRONTEND=noninteractive apt install -y build-essential git libcurl4-gnutls-dev curl libssl-dev

COPY . /workspace/Keyfactor-CAgent
WORKDIR /workspace/Keyfactor-CAgent
RUN make clean
RUN make opentest -j8

FROM ubuntu

RUN apt update -y && DEBIAN_FRONTEND=noninteractive apt install -y libcurl4-gnutls-dev libssl-dev
COPY --from=builder /workspace /workspace
RUN mkdir -p /home/keyfactor/Keyfactor-CAgent/certs/
COPY ./mayhem/config.json /home/keyfactor/Keyfactor-CAgent/
COPY ./mayhem/trust.store /home/keyfactor/Keyfactor-CAgent/certs/
WORKDIR /home/keyfactor/Keyfactor-CAgent

ENTRYPOINT ["/workspace/Keyfactor-CAgent/agent", "-c", "/home/keyfactor/Keyfactor-CAgent/certs/config.json"]
