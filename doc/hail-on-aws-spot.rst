.. _hail-on-aws-spot:

==========================
Hail on AWS Spot Instances
==========================

This package installs the genomics application `Hail <https://hail.is/>`_ along with EMR.  It is developed by Harvard DBMI and hosted on `GitHub <https://github.com/hms-dbmi/hail-on-AWS-spot-instances>`_. It is downloaded and installed onto the instance building the EMR AMI as described in the section :ref:`ami-image`.  

The Git repository is cloned during the execution the script ``provision-hail.sh`` in the building of the AMI, as described in :ref:`provisioning-script`. In order to use the repository, it must be forked to a new location, modified, and the modified version used in the AMI build.

In your fork of the repo, make the following changes in the ``src/`` directory.

.. _bootstrap-python36:

---------------------
bootstrap_python36.sh
---------------------

Create a new file ``bootstrap_python3.sh`` with the contents from the provided file.  This script is similar to the Hail repo's `bootstrap_python36.sh <https://github.com/hms-dbmi/hail-on-AWS-spot-instances/blob/master/src/bootstrap_python36.sh>`_ but refactored and does not use the deprecated Python 3.6.  This script is called during AMI build in the step :ref:`provisioning-script`.

:download:`bootstrap_python3.sh <./bootstrap_python3.sh>`

-------------
hail_build.sh
-------------

Make the following edits.

`Line 45 <https://github.com/hms-dbmi/hail-on-AWS-spot-instances/blob/b9d6addd91e1f1c3a042011b027010ae066e6415/src/hail_build.sh#L45>` ::

    -    HAIL_VERSION="master"
    -    SPARK_VERSION="2.4.0"
    +    HAIL_VERSION="main"
    +    SPARK_VERSION="3.1.2"

`Line 86 <https://github.com/hms-dbmi/hail-on-AWS-spot-instances/blob/b9d6addd91e1f1c3a042011b027010ae066e6415/src/hail_build.sh#L86>` ::

    -    sudo yum install g++ cmake git -y
    -    sudo yum -y install gcc72-c++ # Fixes issue with c++14 incompatibility in Amazon Linux
    +    sudo yum install -y gcc-c++ cmake git
    +    # sudo yum -y install gcc72-c++ # Fixes issue with c++14 incompatibility in Amazon Linux

`Line 131 <https://github.com/hms-dbmi/hail-on-AWS-spot-instances/blob/b9d6addd91e1f1c3a042011b027010ae066e6415/src/hail_build.sh#L131>` ::

    -    else  ./gradlew -Dspark.version=$SPARK_VERSION -Dbreeze.version=0.13.2 -Dpy4j.version=0.10.6 shadowJar archiveZip
    +    else  ./gradlew -Dspark.version=$SPARK_VERSION shadowJar archiveZip


