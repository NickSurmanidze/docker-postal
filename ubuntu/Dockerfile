FROM ruby:2.4

RUN apt-get -y update \
	&& apt-get -y install --no-install-recommends nodejs mysql-client git-core python-minimal python-pip python-dev libcap2-bin python-setuptools \
	&& pip install j2cli \
        && git clone https://github.com/atech/postal.git /opt/postal \
	&& rm -rf /var/lib/apt/lists/* \
	&& gem install bundler \
	&& gem install procodile \
	&& gem install tzinfo-data \
	&& useradd -r -d /opt/postal -s /bin/bash postal \
	&& chown -R postal:postal /opt/postal/ \
	&& /opt/postal/bin/postal bundle /opt/postal/vendor/bundle \
	&& apt-get -y purge python-dev git-core \
	&& apt-get -y autoremove \
	&& apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## Adjust permissions
RUN setcap 'cap_net_bind_service=+ep' /usr/local/bin/ruby

## Stick in required files
ADD src/docker-entrypoint.sh /docker-entrypoint.sh
ADD src/templates /templates

## Expose
EXPOSE 5000

## Startup
ENTRYPOINT ["/bin/bash", "-c", "/docker-entrypoint.sh ${*}", "--"]
