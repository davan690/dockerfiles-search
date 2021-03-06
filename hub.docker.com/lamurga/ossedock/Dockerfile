FROM debian:jessie
MAINTAINER D.H. Bahr <dhbahr@gmail.com>

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            nodejs \
            npm \
            python-support \
            python-pyinotify \
            openssh-client \
        && npm install -g less less-plugin-clean-css \
        && ln -s /usr/bin/nodejs /usr/bin/node \
        && curl -o wkhtmltox.deb -SL http://nightly.odoo.com/extra/wkhtmltox-0.12.1.2_linux-jessie-amd64.deb \
        && echo '40e8b906de658a2221b15e4e8cd82565a47d7ee8 wkhtmltox.deb' | sha1sum -c - \
        && dpkg --force-depends -i wkhtmltox.deb \
        && apt-get -y install -f --no-install-recommends \
        && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false npm \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb

# Install Odoo
ENV ODOO_VERSION 8.0
ENV ODOO_RELEASE 20160419
RUN set -x; \
        curl -o odoo.deb -SL http://nightly.odoo.com/${ODOO_VERSION}/nightly/deb/odoo_${ODOO_VERSION}.${ODOO_RELEASE}_all.deb \
        && dpkg --force-depends -i odoo.deb \
        && apt-get update \
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* odoo.deb

RUN set -x; \
    apt-get update \
    && apt-get install -y --no-install-recommends \
       python-oauthlib python-openssl python-ndg-httpsclient python-pyasn1 python-pip git-core \
    && pip install inflect Geraldo openerp-client-lib

# Remove original l10n_pe, which is just wrong
RUN rm -rf /usr/lib/python2.7/dist-packages/openerp/addons/l10n_pe

# Download Odoo SaaS Tools Addons
RUN git clone -b upstream https://github.com/kaerdsar/odoo-saas-tools.git /mnt/odoo-saas-tools
RUN chown -R odoo. /mnt/odoo-saas-tools/

# Prepare Odoo custom addons
RUN mkdir -p /mnt/odoo-custom-addons/
RUN chown -R odoo. /mnt/odoo-custom-addons/
RUN chmod -R +r /mnt/odoo-custom-addons/

RUN mkdir -p /mnt/odoo-custom-repos/
RUN chown -R odoo. /mnt/odoo-custom-repos/
RUN chmod -R +r /mnt/odoo-custom-repos/

# Add Odoo Docker Addons
COPY addons /mnt/odoo-saas-docker/
RUN chown -R odoo. /mnt/odoo-saas-docker/

# Update Odoo Conf
COPY conf/openerp-server.conf /etc/odoo/
RUN chown odoo /etc/odoo/openerp-server.conf

# Copy python script
COPY makedb.py /etc/odoo/
RUN chown odoo /etc/odoo/makedb.py

# Copy entrypoint script
COPY ./entrypoint.sh /

# Expose Odoo services
EXPOSE 8069 8071 8072

# Set the default config file
ENV OPENERP_SERVER /etc/odoo/openerp-server.conf

ADD ssh/ /var/lib/odoo/.ssh/
RUN chown -R odoo. /var/lib/odoo/.ssh
RUN chmod 600 /var/lib/odoo/.ssh/*

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["openerp-server"]
