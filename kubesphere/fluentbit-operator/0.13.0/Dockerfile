# Please use "make image" to build the docker image
FROM lcr.loongnix.cn/library/golang:1.19-alpine as builder

WORKDIR /workspace

# Copy the Go Modules manifests
COPY go.mod go.mod
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer
RUN set -x \
	&& go env -w GOSUMDB=off \
	&& go mod download 

# Copy the go source
COPY main.go main.go
COPY api api/
COPY controllers controllers/
COPY pkg pkg/

RUN go mod tidy && go get -u golang.org/x/sys/unix && go mod tidy
# Build
RUN CGO_ENABLED=0 GO111MODULE=on go build -a -o manager main.go

# Use distroless as minimal base image to package the manager binary
# Refer to https://github.com/GoogleContainerTools/distroless for more details
FROM lcr.loongnix.cn/library/debian:sid

LABEL maintainer="wangweijie@loongson.cn"

RUN groupadd nonroot \
	&& useradd -g nonroot nonroot
WORKDIR /
COPY --from=builder /workspace/manager .
USER nonroot:nonroot

ENTRYPOINT ["/manager"]
