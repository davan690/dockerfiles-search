FROM golang:1.12-alpine as build
COPY var/ /var/
WORKDIR /usr/locar/go/src/server
COPY main.go main.go
RUN go build -o /server main.go 

FROM alpine:latest
COPY --from=build /var/ /var/
COPY --from=build /server /server
EXPOSE 8080:8080
ENTRYPOINT ["/server"]