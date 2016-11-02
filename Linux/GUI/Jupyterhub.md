<div dir="rtl">بنام خدا</div>

1- Install Python2 , Python3
2- Install Pip2, Pip3
3- Install jupyter : `pip3 install jupyter`
4- Install Jupyterhub : `pip3 install jupyterhub`
5- install some other packages : `pip3 install sudoersspawner `
6- add below to /lib/systemd/system/jupyterhub.service:
  > [Unit]
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
  ```
    c.Authenticator.whitelist = {'test'}
    c.JupyterHub.ssl_key = './server.key'
    c.JupyterHub.ssl_cert = './server.crt'
    #c.JupyterHub.spawner_class = 'sudospawner.SudoSpawner'
    c.JupyterHub.spawner_class = 'systemdspawner.SystemdSpawner'
    c.JupyterHub.ip = 'IP'
    c.JupyterHub.port = Port
    #c.JupyterHub.hub_ip = 'IP'
    #c.JupyterHub.hub_port = Port
    #c.PAMAuthenticator.service = 'login'
    #c.JupyterHub.authenticator_class = 'jupyterhub.auth.PAMAuthenticator'
    #c.PAMAuthenticator.open_sessions = True
  ```
8- and:
  ```
    systemctl daemon-reload
    systemctl enable jupyterhub.service
    systemctl start jupyterhub.service
  ```
9- [Existing Kernel for Jupyter](https://github.com/ipython/ipython/wiki/IPython-kernels-for-other-languages)
  1- install Python2 kernel:
    ```
      python2 -m pip install ipykernel
      python2 -m ipykernel install --user
    ```
  2- [install scala kernel](https://github.com/alexarchambault/jupyter-scala)
    ```
      curl -L -o jupyter-scala https://git.io/vrHhi && chmod +x jupyter-scala && ./jupyter-scala && rm -f jupyter-scala
    ```
  3- [install Apache-toree(for Spark)](https://github.com/apache/incubator-toree)
    ```
    pip install --pre toree
    jupyter toree install
    ```
  4- []()
