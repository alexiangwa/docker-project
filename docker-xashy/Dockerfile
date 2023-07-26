FROM python:3
RUN pip install --upgrade pip
RUN pip install flask
COPY app.py /opt/
ENTRYPOINT  FLASK_APP=app.py flask run --host=0.0.0.0