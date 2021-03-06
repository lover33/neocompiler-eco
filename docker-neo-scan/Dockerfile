FROM nikolauska/phoenix:1.6

RUN apk update && apk upgrade && \
     apk --no-cache --update add git bash

# Here are the default environment variables you can override.
ENV POSTGRES_USERNAME='postgres'
ENV POSTGRES_PASSWORD='postgres'
ENV POSTGRES_DATABASE='neoscan_dev'
ENV POSTGRES_HOSTNAME='postgres'
ENV NEO_SEED_1="http://eco-neo-csharp-nodes-running:30333"
ENV NEO_SEED_2="http://eco-neo-csharp-nodes-running:30334"
ENV NEO_SEED_3="http://eco-neo-csharp-nodes-running:30335"
ENV NEO_SEED_4="http://eco-neo-csharp-nodes-running:30336"
ENV NEO_PYTHON_SEED_1="http://eco-neo-python-running:38088/v1"

EXPOSE 4000

RUN mkdir /neoscan
WORKDIR /neoscan

# NeoCompiler ECO - Last checked commit: e41af7a31ff47b3aa2fbfc7e30f19a79e11ae178 - newer
RUN git clone https://github.com/CityOfZion/neo-scan.git /neoscan && git checkout master && git checkout e41af7a31ff47b3aa2fbfc7e30f19a79e11ae178

RUN mix deps.get
RUN cd /neoscan/apps/neoscan_web/assets && npm install

COPY neoscan_dev.exs /neoscan/apps/neoscan/config/dev.exs
COPY neoscan_sync_config.exs /neoscan/apps/neoscan_sync/config/config.exs
COPY neoscan_monitor_config.exs /neoscan/apps/neoscan_monitor/config/config.exs

# We have to do all this in the command because otherwise the environment variables don't work.
# The sleep is here to make sure postgres is fully started first.
ADD neo-scan-docker-startup.sh /opt
ADD init-neo-scan.sh /opt

CMD /opt/neo-scan-docker-startup.sh
