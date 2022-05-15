FROM golang:alpine as builder

WORKDIR /go/src/app

COPY go.mod .
COPY go.sum .

RUN go mod download

# Get Reflex for live reload in dev
ENV GO111MODULE=on
# Install Reflex for development
RUN go install github.com/cespare/reflex@latest
# RUN go get github.com/cespare/reflex

COPY . .

RUN go build -o ./run .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/

#Copy executable from builder
COPY --from=builder /go/src/app/run .
# COPY --from=builder /go/src/app/docker-entrypoint.sh .

EXPOSE 8080
CMD ["./run"]
#CMD reflex -g '*.go' go run main.go --start-service