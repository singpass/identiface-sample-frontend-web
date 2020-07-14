FROM python:latest
WORKDIR /usr/src/spface/microsvc/face
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY ./Face.py .
COPY ./uat-jws-2020.cer .
CMD [ "python", "./Face.py" ]