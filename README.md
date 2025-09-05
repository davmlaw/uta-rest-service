# uta-rest-service


## INSTALL

    # Fresh Ubuntu 24 VM
    sudo apt update && sudo apt -y upgrade
    sudo apt -y install python3-venv python3-pip nginx git postgresql libpq-dev

    export USER=uta_rest_service
    export DEPLOY_DIR=/opt/uta_rest_service
    export VENV_DIR=${DEPLOY_DIR}/.venv
    sudo adduser --system --group --home /opt/uta_rest_service uta_rest_service
    sudo -u ${USER} git clone https://github.com/davmlaw/uta-rest-service ${DEPLOY_DIR}/.
    sudo -u ${USER} python3 -m venv ${VENV_DIR}
    sudo -u ${USER} ${VENV_DIR}/bin/pip install --upgrade pip wheel
    sudo -u ${USER} ${VENV_DIR}/bin/pip install -r ${DEPLOY_DIR}/requirements.txt

    # FastAPI service
    sudo cp ${DEPLOY_DIR}/config/systemd/uta_rest_service_uvcorn.service /lib/systemd/system
    sudo systemctl daemon-reload
    sudo systemctl enable --now uta_rest_service_uvcorn
    sudo systemctl status --no-pager uta_rest_service_uvcorn

    # Nginx - add site / reload
    sudo cp ${DEPLOY_DIR}/config/nginx/sites-available/uta_rest_service /etc/nginx/sites-available/
    # **NOTE** : you will need to add your IP / DNS to the config file above for nginx routing to work!
    sudo ln -s /etc/nginx/sites-available/uta_rest_service /etc/nginx/sites-enabled/uta_rest_service
    sudo nginx -t && sudo systemctl reload nginx

