FROM golang:1.14-alpine as builder

ARG DOCK_PKG_DIR=/go/src/github.com/kyma-project/kyma/components/application-operator
WORKDIR $DOCK_PKG_DIR

COPY go.mod .
COPY go.sum .
RUN go mod download

COPY ./licenses/ /app/licenses
COPY . $DOCK_PKG_DIR

RUN CGO_ENABLED=0 GOOS=linux go build -a -o manager ./cmd/manager

FROM scratch
LABEL source=git@github.com:kyma-project/kyma.git

COPY charts/application application/
COPY charts/gateway gateway/
COPY --from=builder /go/src/github.com/kyma-project/kyma/components/application-operator/manager .
COPY --from=builder /app/licenses /app/licenses

CMD ["/manager"]
