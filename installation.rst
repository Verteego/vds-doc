############
Installation
############

You'll need to have Ansible running on your local machine to manage the automatic installation process for you.

**Please note that in the following installation instructions we use 2 placeholders for local directories: VDS_ROOT and SSH_ROOT. Just replace them with the correct directories from your system.**

- VDS_ROOT: directory where Verteego Data Suite will be cloned
- SSH_ROOT: current user's .ssh directory path

1. Clone Verteego Data Suite
""""""""""""""""""""""""""""
Clone the following repository to your local machine (NOT the remote server where you want to install Verteego DS, we'll precise this later).

::

    git clone git@bitbucket.org:verteegois/dss.git


2. Install Ansible
""""""""""""""""""
We'll use Ansible to deploy Verteego DS. If you don't have it yet, please install Ansible on your local machine.

**Linux**

http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu

**Mac OS (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

**Windows (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

3. Install Verteego DSS
"""""""""""""""""""""""

**INSTALLATION ON A LOCAL VIRTUAL SERVER**

- install virtualbox : https://www.virtualbox.org/wiki/Downloads
- install vagrant    : https://www.vagrantup.com/docs/installation/
- Go to the vagrant directory (VDS_ROOT/vagrant) and execute (this may take sometime as it will download a full debian image to install on virtualbox):

::

    vagrant up



- Go to ansible directory (VDS_ROOT/vagrant) and execute:
::

    ansible-playbook -i $VDS_ROOT/hosts --private-key=VDS_ROOT/vagrant/.vagrant/machines/dss/virtualbox/private_key $VDS_ROOT/setup_cluster.yml

- Navigate to http://dss.local.verteego:33330



**INSTALLATION ON GOOGLE CLOUD PLATFORM**

**1. Install Google Cloud SDK**

You should have a running Google Cloud platform account and the SDK installed. It this is already the case you can directly proceed to step 2.

- Install GCloud SDK :
    - https://cloud.google.com/sdk/docs/
- configure your account and project:

::
    gcloud init


- generate SSH key for gcloud:

::
     gcloud compute config-ssh


**2. Set up the VDS environment on Google Cloud**
- Create a Google service account :
    - Go to https://console.cloud.google.com/iam-admin/serviceaccounts
    - Select the project into which you want to create the VDS instance
    - Create a service account with project editor role:
    - Check the "Furnish a new private key" option
    - Chose JSON key type
    - Copy the downloaded key file to VDS_ROOT/deployment/ansible/files and rename it to ansible.json
::

     mv Downloads/KEYFILE.json VDS_ROOT/deployment/ansible/files/


    .. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_01.jpeg

    .. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_02.jpeg

- Install libcloud

::
    sudo apt-get install python-pip
    sudo pip install -U apache-libcloud


- launch playbook by going to ansible directory and running :

::
    ansible-playbook -i VDS_ROOT/deployment/ansible/hosts --private-key=SSH_ROOT/google_compute_engine VDS_ROOT/deployment/ansible/setup_gc_instance.yml


- Be patient... Installation can take several minutes depending on the capacity of the server you've chosen.
- Navigate to the newly created instance IP at port 33330. You can find it on on your Google Cloud Compute Engine console: http://VDS_INSTANCE_IP:33330


**LOGIN**

For your first sign in you can use the following credentials. For security reasons, remember to change them or delete the default user after your first login.
Username: dss-user
Password: verteego