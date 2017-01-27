############
Installation
############

You'll need to have Ansible running on your local machine to manage the automatic installation process for you.

**N.B:**

- $VDS_ROOT : directory where verteego data suite is cloned
- $.SSH_ROOT : current user .ssh directory path

1. Clone Verteego data suite
""""""""""""""""""""""""""""
- git clone git@bitbucket.org:verteegois/dss.git

2. Install Ansible
""""""""""""""""""

**Linux**

http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu

**Mac OS (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

**Windows (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

3. Install Verteego DSS
"""""""""""""""""""""""

**Installation on a local virtual server**

- install virtualbox : https://www.virtualbox.org/wiki/Downloads
- install vagrant    : https://www.vagrantup.com/docs/installation/
- Go to directory vagrant ($VDS_ROOT/vagrant) and execute :
::

    vagrant up """this may take sometime as it will download a full debian image to install on virtualbox

- Go to ansible directory and execute:
::

    ansible-playbook -i $VDS_ROOT/hosts --private-key=$VDS_ROOT/vagrant/.vagrant/machines/dss/virtualbox/private_key $VDS_ROOT/setup_cluster.yml

- Navigate to http://dss.local.verteego:33330

**Installation on Google Cloud Platform**

- Install gcloud sdk :
    - https://cloud.google.com/sdk/docs/
- configure your account and project :
::

    gcloud init

- generate ssh key for gcloud :
::

     gcloud compute config-ssh
- Create google service account :
    - go to https://console.cloud.google.com/iam-admin/serviceaccounts
    - select the project into which you want to create a vds instance
    - create service account with project editor role:

    .. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_01.jpeg

    .. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_02.jpeg

    - copy the key file downloaded to VDS_ROOT/files and rename it to ansible.json

- Install libcloud
::

    sudo apt-get install python-pip
    sudo pip install -U apache-libcloud

- launch playbook by going to ansible directory and running :
::

    ansible-playbook -i $VDS_ROOT/hosts --private-key=$.SSH_ROOT/google_compute_engine $VDS_ROOT/setup_gc_instance.yml

- Navigate to the newly created instance ip address at port 33330 : http://gc_instance_ip:33330
