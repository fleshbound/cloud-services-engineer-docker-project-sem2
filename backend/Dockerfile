FROM golang:1.17-alpine AS builder

WORKDIR /app

COPY . .

RUN go build -o server ./cmd/main.go && rm -rf /var/cache/*

EXPOSE 8081

CMD ["./server"] 
