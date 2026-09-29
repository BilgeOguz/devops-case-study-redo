# This Dockerfile was created from the lessons of 
# KodeKloud-Docker Certifies Assosiate Exam Course

FROM alpine:3.24 

RUN apk update && apk add python3 && apk add py3-pip

COPY requirements.txt .
#RUN pip install -r requirements.txt

#FROM python:3.9-slim  

RUN pip install --break-system-packages -r requirements.txt

COPY ./src /src 
WORKDIR /src

EXPOSE 5000  

ENTRYPOINT ["python", "app.py"]
