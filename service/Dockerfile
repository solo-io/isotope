# Note: this image must be built from the root of the repository for access to
# the vendor folder.

FROM golang:1.18.4 AS builder

RUN mkdir /build

COPY . /build/

WORKDIR /build

RUN  --mount=type=cache,target=/go/pkg/mod \
  --mount=type=cache,target=/root/.cache/go-build \
  GOARCH=$BUILDPLATFORM CGO_ENABLED=0 GOOS=linux go build -o isotope_service ./service

FROM alpine:3.12

COPY --from=builder /build/isotope_service /usr/local/bin/isotope_service

EXPOSE 8080

ENTRYPOINT ["/usr/local/bin/isotope_service"]
