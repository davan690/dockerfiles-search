FROM alpine:latest as certs
RUN apk --update add ca-certificates

FROM scratch

COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

VOLUME /srv
EXPOSE 80

COPY filebrowser.json /.filebrowser.json
COPY filebrowser.linux_amd64 /filebrowser

ENTRYPOINT [ "/filebrowser" ]
