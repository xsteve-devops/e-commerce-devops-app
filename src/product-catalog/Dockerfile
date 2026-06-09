FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app


COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o product-catalog . 

FROM alpine AS release

WORKDIR /usr/src/app 

COPY --from=builder /usr/src/app/product-catalog ./

ENV PRODUCT_CATALOG_PORT=8088

COPY ./products ./products

ENTRYPOINT ["./product-catalog"]

