FROM python:3.9

EXPOSE 8080
USER root

ENV INSTANA_SERVICE_NAME=payment

WORKDIR /app

# Install build dependencies for uwsgi
RUN apt-get update && apt-get install -y \
    build-essential \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt /app/

# Install requirements
RUN pip install --no-cache-dir -r requirements.txt

COPY *.py /app/
COPY payment.ini /app/

# uWSGI needs to run the app
CMD ["uwsgi", "--ini", "payment.ini"]