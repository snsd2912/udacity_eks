FROM python:3.10-slim

WORKDIR /src

COPY ./app/. .

RUN apt-get update -y && \
    apt-get -y install build-essential libpq-dev && \
    pip install --upgrade pip setuptools wheel

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
