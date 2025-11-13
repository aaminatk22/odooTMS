FROM odoo:19.0

USER root

# Install system dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    build-essential \
    python3-dev \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Copy the entire odoo-server directory
COPY odoo-server /opt/odoo/odoo-server

# Copy and install requirements
COPY odoo-server/requirements.txt /tmp/requirements.txt
RUN pip3 install -r /tmp/requirements.txt

# Create symbolic links for custom addons
RUN mkdir -p /mnt/extra-addons
RUN ln -sf /opt/odoo/odoo-server/addons/* /mnt/extra-addons/ 2>/dev/null || true
RUN ln -sf /opt/odoo/odoo-server/dh_contact /mnt/extra-addons/ 2>/dev/null || true
RUN ln -sf /opt/odoo/odoo-server/dh_tms /mnt/extra-addons/ 2>/dev/null || true

# Set proper permissions
RUN chown -R odoo:odoo /opt/odoo/odoo-server
RUN chown -R odoo:odoo /mnt/extra-addons

USER odoo

# Use the odoo-bin from your project
CMD ["/opt/odoo/odoo-server/odoo-bin", "--config=/etc/odoo/odoo.conf"]