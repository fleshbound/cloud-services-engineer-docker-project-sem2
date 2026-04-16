ARG APP_VERSION=1.0.0
ARG GO_VERSION=1.25

# Build stage
FROM golang:${GO_VERSION}-alpine AS builder

ARG APP_VERSION

WORKDIR /app

COPY go.mod go.sum .

RUN go mod download && \
    go mod verify

COPY . .

RUN CGO_ENABLED=0 go build -ldflags="-w -s -X main.version=${APP_VERSION}" \
    -o server ./cmd/api/

# Final stage (run server)
FROM alpine:3.23

ARG APP_VERSION
LABEL version="${APP_VERSION}"

WORKDIR /app

RUN addgroup -S appuser && \
    adduser -S -s /sbin/nologin -G appuser appuser

COPY --from=builder --chown=appuser:appuser /app/server .

USER appuser

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8081/health || exit 1

CMD ["./server"]
