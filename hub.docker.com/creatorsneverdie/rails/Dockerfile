FROM ruby:latest

# install node, yarn, mysql-client, pg-client
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y nodejs yarn mysql-client postgresql-client ruby-dev gcc && \
    gem install rails && \
    rm -rf /var/lib/apt/lists/*
