# builder for installing python packages
FROM python:3.9.7-buster as builder

# install mlflow and psycopg2
RUN pip install \
    mlflow \
    psycopg2 \
    boto3

# main image
FROM python:3.9.7-slim-buster

# copy python packages
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
# copy mlflow package
COPY --from=builder /usr/local/bin/mlflow /usr/local/bin/mlflow
# copy linux packages for postgres
COPY --from=builder /usr/lib/aarch64-linux-gnu /usr/lib/aarch64-linux-gnu
COPY --from=builder /lib/aarch64-linux-gnu /lib/aarch64-linux-gnu
# copy gunicorn package
COPY --from=builder /usr/local/bin/gunicorn /usr/local/bin/gunicorn

# open port 5001
EXPOSE 5001

# set environment variables
ARG POSTGRES_USER
ARG POSTGRES_PASSWORD
ARG DB_HOST
ARG DB_NAME
ARG MLFLOW_S3_ENDPOINT_URL
ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY
ARG DEFAULT_ARTIFACT_ROOT
ENV POSTGRES_USER=${POSTGRES_USER}
ENV POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
ENV DB_HOST=${DB_HOST}
ENV DB_NAME=${DB_NAME}
ENV MLFLOW_S3_ENDPOINT_URL=${MLFLOW_S3_ENDPOINT_URL}
ENV AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
ENV AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
ENV DEFAULT_ARTIFACT_ROOT=${DEFAULT_ARTIFACT_ROOT}

# Start mlflow server
CMD mlflow server \
    --host 0.0.0.0 \
    --port 5001 \
    --backend-store-uri postgresql+psycopg2://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${DB_HOST}:5432/${DB_NAME} \
    --default-artifact-root ${DEFAULT_ARTIFACT_ROOT}
