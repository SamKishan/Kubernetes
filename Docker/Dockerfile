FROM python:2.7-onbuild
WORKDIR /usr/src/app
EXPOSE 5000
COPY requirements.txt ./
RUN pip	install --no-cache-dir -r requirements.txt 
COPY . .
CMD [ "python", "app.py"]
