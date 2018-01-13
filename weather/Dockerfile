FROM golang:latest as build-miot-weather
ADD . /go/src/github.com/kapitanov/miot-weather
WORKDIR /go/src/github.com/kapitanov/miot-weather
RUN go get
RUN CGO_ENABLED=0 go build -o miot-weather . 

FROM golang:latest as build-yandex-weather-cli
RUN CGO_ENABLED=0 go get -u github.com/msoap/yandex-weather-cli

FROM alpine:latest
RUN apk --no-cache add ca-certificates

COPY --from=build-miot-weather /go/src/github.com/kapitanov/miot-weather/miot-weather /app/miot-weather
COPY --from=build-miot-weather /go/src/github.com/kapitanov/miot-weather/www /app/www

COPY --from=build-yandex-weather-cli /go/bin/yandex-weather-cli /bin

EXPOSE 3000
WORKDIR /app
HEALTHCHECK --start-period=30s CMD curl -f http://localhost:3000/api/weather || exit 1
CMD ["/app/miot-weather"]