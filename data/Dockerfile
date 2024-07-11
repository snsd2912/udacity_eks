FROM python:3.10-slim-buster

WORKDIR /src

COPY ./app/. .

RUN pip install -r requirements.txt

RUN ls 

CMD ["python", "./app.py"]
