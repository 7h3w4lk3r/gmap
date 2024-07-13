
# Now you can point your browser to http://localhost:8000

FROM ubuntu:latest

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
    python3 python3-pip python3-venv curl wget git wkhtmltopdf libssl-dev vim nmap tzdata

RUN mkdir /opt/xml && mkdir /opt/notes && \
    wget -P /opt/ https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz && \
    cd /opt/ && tar -xvf /opt/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz

RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --upgrade pip && \
    pip install Django requests xmltodict

RUN cd /opt/ && django-admin startproject nmapdashboard && cd /opt/nmapdashboard && \
    git clone https://github.com/7h3w4lk3r/gmap.git nmapreport && \
    cd nmapreport && git checkout master

RUN cp /opt/nmapdashboard/nmapreport/docker/settings.py /opt/nmapdashboard/nmapdashboard/
RUN cp /opt/nmapdashboard/nmapreport/docker/urls.py /opt/nmapdashboard/nmapdashboard/
RUN cp /opt/nmapdashboard/nmapreport/docker/tzdata.sh /root/tzdata.sh
RUN cp /opt/nmapdashboard/nmapreport/docker/startup.sh /startup.sh

RUN cd /opt/nmapdashboard && python3 manage.py migrate
RUN apt-get autoremove -y
RUN ln -s /opt/nmapdashboard/nmapreport/token.py /root/token
RUN chmod +x /root/token

EXPOSE 8000

ENTRYPOINT ["bash", "/startup.sh"]
