sudo chown -R shopapi:www-data /opt/chicaiza-noche-shopapi/staticfiles
sudo chmod -R 755 /opt/chicaiza-noche-shopapi/staticfiles
sudo chmod -R 755 /opt/chicaiza-noche-shopapi


[Unit]
Description=Gunicorn daemon for ShopAPI
After=network.target postgresql.service

[Service]
User=shopapi
Group=www-data
WorkingDirectory=/opt/chicaiza-noche-shopapi
Environment="PATH=/opt/chicaiza-noche-shopapi/.venv/bin"
EnvironmentFile=/opt/chicaiza-noche-shopapi/.env
ExecStart=/opt/shopapi/.venv/bin/gunicorn \
          --workers 3 \
          --bind unix:/run/gunicorn-shopapi.sock \
          --access-logfile /var/log/gunicorn-shopapi-access.log \
          --error-logfile /var/log/gunicorn-shopapi-error.log \
          config.wsgi:application
ExecReload=/bin/kill -s HUP $MAINPID
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target