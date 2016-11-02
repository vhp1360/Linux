<div dir="rtl">بنام خدا</div>

1- Install Python2 , Python3
2- Install Pip2, Pip3
3- Install jupyter : `pip3 install jupyter`
4- Install Jupyterhub : `pip3 install jupyterhub`
5- install some other packages : `pip3 install sudoersspawner `
6- add below to /lib/systemd/system/jupyterhub.service:
  >[Unit]
   Description=Jupyterhub
   After=network-online.target
  >
   [Service]
   User=root
   group=root
   ExecStart=/usr/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py --debug
   WorkingDirectory=/etc/jupyterhub
  >
   [Install]
   WantedBy=multi-user.target
   
7- some parameters in /etc/jupyterhub/jupyterhub/jupyterhub_config.py:
