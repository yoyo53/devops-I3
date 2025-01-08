# syntax=docker/dockerfile:1

FROM golang:1.21-alpine 

WORKDIR /app

COPY go.mod ./
RUN go mod download

COPY *.go ./

RUN CGO_ENABLED=0 GOOS=linux go build -o /docker-gs-ping

EXPOSE 8085

# Run
CMD ["/docker-gs-ping"]