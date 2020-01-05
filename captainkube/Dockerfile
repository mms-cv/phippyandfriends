# Build
FROM golang:latest as builder

ENV KUBERNETES_SERVICE_HOST = "kubernetes.cluster.local"
ENV KUBERNETES_SERVICE_PORT = 443

WORKDIR /build
COPY main.go .
RUN go get -d -v
# temporary workaroud for https://github.com/Azure/phippyandfriends/issues/30
RUN cd $GOPATH/src/k8s.io/klog && git checkout v0.4.0 && cd -
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

# Run
FROM alpine:latest  

RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /build/app .
CMD ["./app"]
