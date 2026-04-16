# Build stage
FROM golang:1.25-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download && \
    go mod verify

COPY . .

RUN CGO_ENABLED=0 go build -ldflags="-w -s -extldflags '-static'" \
    -o server ./cmd/main.go

# Final stage (run server)
FROM alpine:3.19

WORKDIR /app

RUN addgroup -S appgroup && \
    adduser -S -s /sbin/nologin -G appgroup appuser

COPY --from=builder --chown=appuser:appgroup /app/server .

USER appuser

EXPOSE 8081

RUN apk add --no-cache curl

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD curl -s http://localhost:8081/health || exit 1

CMD ["/server"]
