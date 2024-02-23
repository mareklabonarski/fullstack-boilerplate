FROM python:3.11.1-slim

WORKDIR /backend

RUN apt-get update
RUN apt-get install libpq-dev python-dev -y
RUN apt-get install python3-pip -y

# set env variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install dependencies
RUN python -m pip install pip --upgrade
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY backend /backend
