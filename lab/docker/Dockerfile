FROM golang:1.24.5-bookworm AS builder
WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o /out/gobgpd ./cmd/gobgpd && \
    CGO_ENABLED=0 go build -o /out/gobgp ./cmd/gobgp

FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /out/gobgpd /usr/local/bin/gobgpd
COPY --from=builder /out/gobgp /usr/local/bin/gobgp

ENTRYPOINT ["gobgpd"]
