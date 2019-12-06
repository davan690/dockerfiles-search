FROM python:3.7-buster

WORKDIR /usr/src/yesses

COPY requirements.txt ./
RUN apt update && apt install -y nmap
RUN pip install requests && pip install --no-cache-dir -r requirements.txt

COPY . .

ENTRYPOINT [ "python", "/usr/src/yesses/run.py" ]