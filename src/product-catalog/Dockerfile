FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o product-catalog .


FROM alpine:3.22 AS release
WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/product-catalog .
COPY ./products ./products
ENV PRODUCT_CATALOG_PORT=8088

RUN adduser -D -u 10001 appuser
USER appuser

ENTRYPOINT ["./product-catalog"]