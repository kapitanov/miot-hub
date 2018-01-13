FROM golang:latest as build
ADD . /go/src/github.com/kapitanov/miot-time
WORKDIR /go/src/github.com/kapitanov/miot-time
RUN go get
RUN CGO_ENABLED=0 go build -o miot-time . 

FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=build /go/src/github.com/kapitanov/miot-time/miot-time /app/miot-time
COPY --from=build /go/src/github.com/kapitanov/miot-time/www /app/www
COPY --from=build /usr/local/go/lib /usr/local/go/lib
EXPOSE 3000
WORKDIR /app
HEALTHCHECK --start-period=30s CMD curl -f http://localhost:3000/api/time || exit 1
CMD ["/app/miot-time"]