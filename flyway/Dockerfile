
# Regular
FROM flyway/flyway:latest-alpine

WORKDIR /flyway

COPY ./migrations ./migrations
COPY ./conf ./conf


# For RDS + SSL/TLS: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html
# FROM flyway/flyway:latest

# RUN apt-get update \
#     && apt-get install -y --no-install-recommends curl ca-certificates \
#     && rm -rf /var/lib/apt/lists/*

# ENV CERT_FILE_PATH=/etc/ssl/certs/rds-global-bundle.pem

# RUN curl -fsSL https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem \
#     -o ${CERT_FILE_PATH}

# WORKDIR /flyway

# COPY ./migrations ./migrations
# COPY ./conf ./conf
