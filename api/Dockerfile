FROM golang:1.26-alpine AS build

WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 go build -trimpath -ldflags="-s -w" -o /out/api ./cmd/api

FROM alpine:3.22

RUN apk add --no-cache ca-certificates \
    && adduser -D -H -u 10001 appuser
WORKDIR /app
COPY --from=build --chown=appuser:appuser /out/api /usr/local/bin/api

USER appuser
EXPOSE 8080
ENTRYPOINT ["/usr/local/bin/api"]