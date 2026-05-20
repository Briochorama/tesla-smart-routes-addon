FROM golang:1.23-alpine AS builder
RUN apk add --no-cache git
WORKDIR /build
RUN git clone https://github.com/teslamotors/vehicle-command.git . && \
    git fetch origin pull/443/head:pr-443 && \
    git checkout pr-443 && \
    go build -o tesla-http-proxy ./cmd/tesla-http-proxy

FROM alpine:3.19
RUN apk add --no-cache openssl
COPY --from=builder /build/tesla-http-proxy /usr/local/bin/tesla-http-proxy
COPY run.sh /run.sh
RUN chmod +x /run.sh
EXPOSE 4443
CMD ["/run.sh"]
