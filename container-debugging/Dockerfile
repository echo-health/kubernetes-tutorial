FROM golang:1.13-alpine AS build
WORKDIR /goroutine
COPY go.mod /goroutine/go.mod
COPY go.sum /goroutine/go.sum
RUN go mod download
COPY . /goroutine

# CGO_ENABLED=0 forces a single static unlinked binary
# -ldflags="-w -s" means remove the symbol and DWARF table which makes the binary size smaller (used for debugger which we won't be using)
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o build/goroutine . 


FROM scratch
COPY --chown=0:0 --from=build /goroutine/build/goroutine /go/bin/goroutine
ENTRYPOINT ["/go/bin/goroutine"]