.. _ami-image:

=========
AMI Image
=========

This section describes building the custom AMI image from which the master and core EC2 nodes are launched. All the software used by the EMR workspace is built into the AMI.  To upgrade the version of the EMR workspace type, two changes are necessary: the AMI image definition, and the provisioning script.

-----------------------------------------------
AMI Image Definition: packer-emr-workspace.json
-----------------------------------------------

The configuration of the AMI image is defined in the file ``main/solution/machine-images/config/infra/packer-emr-workspace.json``.  This file is used as input to Packer, which builds the AMI.

Change the OS of the building instance from Amazon Linux 1 to Amazon Linux 2.  This is to upgrade the C++ compiler from v4.8.5 to v7.3.1

`Line 23 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/packer-emr-workspace.json#L23>`_ ::

    -          "name": "amzn-ami-hvm-*-x86_64-ebs",
    +          "name": "amzn2-ami-hvm-*-x86_64-gp2",

.. _provisioning-script:

--------------------------------------
Provisioning Script: provision-hail.sh
--------------------------------------

The Packer configuration file ``packer-emr-workspace.json`` calls the Service Workbench script ``provision-hail.sh`` to install the software needed by the cluster:

`Line 37 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/packer-emr-workspace.json#L37>`_ ::

  "provisioners": [
    {
      "type": "shell",
      "script": "./config/infra/provisioners/provision-hail.sh"
    }
  ]

Make the following edits to ``provision-hail.sh``:

`Line 10 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/provisioners/provision-hail.sh#L10>`_ ::

    git clone https://github.com/hms-dbmi/hail-on-AWS-spot-instances.git /opt/hail-on-AWS-spot-instances

Change this to the URL of your edited version of Hail On AWS Spot Instances, as created in :ref:`hail-on-aws-spot`.

`Line 21 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/provisioners/provision-hail.sh#L21>`_.  We will supply a different script to call in the step :ref:`bootstrap-python36` ::

    -   sudo ./bootstrap_python36.sh
    +   sudo ./bootstrap_python3.sh

`Line 24 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/provisioners/provision-hail.sh#L24>`_.  Change the PACKAGES declaration to the following:  ::

    PACKAGES="aiohttp==3.8.3
    aiohttp-session==2.12.0
    aiohttp-session==2.12.0
    asyncinit==0.2.4
    bokeh==2.4.3
    gcsfs==2022.11.0
    humanize==4.4.0
    hurry.filesize==0.9
    jupyterlab==3.5.2
    nest-asyncio==1.5.6
    pandas==1.3.5
    PyJWT==2.6.0
    pyspark==3.3.1
    python-json-logger==2.0.4
    requests==2.28.1
    requests-oauthlib==1.3.1
    scipy==1.7.3
    tabulate==0.9.0
    tqdm==4.64.1"

`Line 43 <https://github.com/awslabs/service-workbench-on-aws/blob/49f46df7598f1f19e9d950db6a952797186d5fbe/main/solution/machine-images/config/infra/provisioners/provision-hail.sh#L43>`_ ::

    -    sudo python3 -m pip install -Iv jupyterlab==2.2.6
    +    sudo python3 -m pip install -Iv jupyterlab==3.5.2

