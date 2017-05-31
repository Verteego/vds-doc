############
Ansible
############

**Please note that in the following installation instructions we use 2 placeholders for local directories: VDS_ROOT and SSH_ROOT. Just replace them with the correct directories from your system.**

- VDS_ROOT: local directory where the installation package will be cloned to
- SSH_ROOT: local .ssh directory


1. Install Ansible
""""""""""""""""""
We'll use Ansible to deploy Verteego DS to your remote server or Virtualbox. If you don't have Ansible yet, please install it as we'll use Ansible to orchestrate the automatic installation process for you.

**Linux**

http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu

**Mac OS (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

**Windows (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html


2. Clone installation package
"""""""""""""""""""""""""""""
Clone the following repository to your local machine (NOT to the remote server on which you want run Verteego DS). We'll call this repository VDS_ROOT in the following.

::

    git clone https://github.com/Verteego/vds.git


3. Install Verteego DS
""""""""""""""""""""""

3.1 Installation on Amazon web service
......................................

**1. Configure account and fetch needed authentication file**

- Create a key-pair and call it vds:

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/create-key-pair-1.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/create-key-pair-2.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/create-key-pair-3.jpeg
    :scale: 50%

- Change the right of the downloaded file (vds.pem) to 400 :

::

    chmod 0400 Downloads/vds.pem


- Copy it to
::

    cp Downloads/vds.pem VDS_ROOT/deployment/ansible/files/aws/vds.pem


- Configure account rights :

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-1.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-2.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-3.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-4.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-5.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-6.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-right-7.jpeg
    :scale: 50%

- Create Access/Secret keys :

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-key-1.jpeg
    :scale: 50%

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/account-key-2.jpeg
    :scale: 50%

- Copy the Access and secret keys into key.json file under VDS_ROOT/deployment/ansible/files/aws/keys.json

**2. Launch installation**

::

    ansible-playbook -i VDS_ROOT/deployment/ansible/hosts --private-key=VDS_ROOT/deployment/ansible/files/aws/vds.pem -u admin VDS_ROOT/deployment/ansible/setup_on_aws.yml


3.2 Installation on google cloud platform
.........................................

**1. Install Google Cloud SDK**

Before you start you should make sure that you have a running Google Cloud platform account and the GCloud SDK installed (to install GCloud SDK: https://cloud.google.com/sdk/docs).

- Configure your account and project

::

    gcloud init



- Generate SSH key for GCloud

::

    gcloud compute config-ssh


**2. Set up the VDS environment on Google Cloud**

- Create a Google service account :
    - Go to https://console.cloud.google.com/iam-admin/serviceaccounts
    - Select the project into which you want to create the VDS instance
    - Create a service account with project editor role
    - Check the "Furnish a new private key" option
    - Chose JSON key type
    - When you click the "Create" button, a key file will be the downloaded. Copy the downloaded key file to VDS_ROOT/deployment/ansible/files/gcp and rename it to ansible.json

::

     cp Downloads/ORIGINAL_KEYFILE.json VDS_ROOT/deployment/ansible/files/gcp/ansible.json



.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/step_01.jpeg
    :scale: 50%


.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/step_02.jpeg
    :scale: 50%


**3. Install libcloud**

::

    sudo apt-get install python-pip
    sudo pip install apache-libcloud==1.5.0

    # in case you encounter an ssl certificate validation issue (https://libcloud.readthedocs.io/en/latest/other/ssl-certificate-validation.html#ssl-certificate-validation-in-v2-0)
    sudo pip install --upgrade certifi


**4. Launch installation**

This will launch the default installation of Verteego Data Suite. For custom settings such as instance calibration, read `this <#custom-settings>`_.

::

    ansible-playbook -i VDS_ROOT/deployment/ansible/hosts --private-key=SSH_ROOT/google_compute_engine VDS_ROOT/deployment/ansible/setup_gc_instance.yml


- Be patient, the deployment of all files can take a while depending on the capacity of the instance you've chosen.


**5. Start playing**

- When the installation process has finished, using a browser, navigate to the newly created instance external IP on port 33330 : http://GC_INSTANCE_EXTERNAL_IP:33330.
You can find the external ip address on on your Google Cloud Compute Engine web page console (https://console.cloud.google.com/compute/instances).


3.3 Installation on a local virtual server (virtualbox) from sources
....................................................................


**1. Install Virtualbox and Vagrant**

- Install Virtualbox: https://www.virtualbox.org/wiki/Downloads

- Install Vagrant: https://www.vagrantup.com/docs/installation


**2. Launch Vagrant**

- Go to the Vagrant directory (VDS_ROOT/vagrant) and launch Vagrant (this may take a while as it will download a full Debian image to be installed on Virtualbox):

::

    cd VDS_ROOT/vagrant
    vagrant up

**3. Installation**

- Launch installation

::

    ansible-playbook -i VDS_ROOT/deployment/ansible/hosts --private-key=VDS_ROOT/vagrant/.vagrant/machines/vds/virtualbox/private_key VDS_ROOT/deployment/ansible/setup_on_vbox.yml



**4. Start playing**

- Navigate to http://VIRTUALBOX_INSTANCE_IP:33330


3.4 Installation on a remote virtual private server (vps)
.........................................................

**Requirements :**

- this playbook is designed to work on a **debian 8** distribution, so we assume your VPS to be running a debian 8
- you should be able to connect o you VPS using a private key **without password**
- you should know your VPS's public ip
- remote user should be part of group **sudoer**, because we need sudo privileges to run all commands
- your server should **expose the port range 33330 to 33335**, to enable external access to the verteego datasuite.


**1. Install VDS**
::
    # Pay attention to the comma after the VPS_PUBLIC_IP
    ansible-playbook \
    -i 'VPS_PUBLIC_IP,' \
    --private-key=PATH_TO_VPS_PRIVATE_SSH_KEY \
    -u REMOTE_USER \
    VDS_ROOT/setup_on_vps.yml

**2. Start playing**

- Navigate to http://VPS_PUBLIC_IP:33330

3. Sign in
""""""""""

For your first sign in you can use the following credentials. For security reasons, remember to change them or delete the default user after your first login.

- Username: vds-user

- Password: verteego


4. Custom settings
""""""""""""""""""

**Customize infrastructure settings**

Your installation can be easily customised using the different .YML files in the VDS_ROOT/deployment/ansible directory.

Example: Use a high-memory instance on Google Cloud

- Open VDS_ROOT/deployment/ansible/setup_gc_instance.yml
- In the vars:machine_type variable replace n1-standard-1 with n1-highmem-16. (see https://cloud.google.com/compute/docs/machine-types)

You can also directly precise specific settings in the command line using the --extra-vars parameter while running ansible-playbook.

Example : Use a high-memory instance on Google Cloud and deploy instance in a different zone

::

    ansible-playbook \n
    -i VDS_ROOT/deployment/ansible/hosts \n
    --private-key=SSH_ROOT/google_compute_engine VDS_ROOT/deployment/ansible/setup_gc_instance.yml
    --extra-vars "ginstance_type=n1-highmem-16 gzone=us-central1-f"



**Customize application settings**

Open VDS_ROOT/deployment/ansible/group_vars/all/vars_file.yml to change the default settings for the different applications composing Verteego Data Suite.