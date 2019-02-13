FROM camptocamp/odoo-project:11.0-latest

RUN DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y git libxmlsec1-dev pkg-config python-dev python3-dev && \
    apt-get install -y libxml2-dev libxslt1-dev libssl-dev libffi-dev build-ess$
    apt-get clean
  
COPY . /odoo
RUN pip3 install -r /odoo/requirements.txt
RUN pip3 install -e /odoo
RUN pip3 install -e /odoo/src


ENV LIST_DB=True
#ENV DEMO=False
ENV LIMIT_TIME_CPU=60000
ENV LIMIT_TIME_REAL=120000
ENV ADDONS_PATH=/odoo/local-src,/odoo/src/addons,/odoo/links,/odoo/external-src/server-tools,/odoo/external-src/odoo-brasil
