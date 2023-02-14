FROM docker.io/golang:latest AS hello-world-hahaha

WORKDIR /app

COPY hello-world.go /app

RUN go build hello-world.go

ENTRYPOINT ["./hello-world"]
