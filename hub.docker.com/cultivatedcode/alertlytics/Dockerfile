#--- Building release
FROM elixir:1.8-alpine as build

WORKDIR /app
COPY . /app
RUN rm -rf /app/_build

RUN apk add --no-cache bash
RUN mix local.hex --force && \
    mix local.rebar --force

ENV MIX_ENV=prod

RUN mix deps.get --force && \
    mix deps.compile

RUN mix release 

#--- Creating production container
FROM elixir:1.8-alpine

RUN apk add --update bash openssl
RUN mkdir /app && chown -R nobody: /app

WORKDIR /app
USER nobody

COPY --from=build /app/_build/prod/rel/alertlytics ./

VOLUME /etc/alertlytics
ENV REPLACE_OS_VARS=true

#Set default entrypoint and command
ENTRYPOINT ["/app/bin/alertlytics"]
CMD ["foreground"]