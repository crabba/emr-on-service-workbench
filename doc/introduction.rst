============
Introduction
============

This is a guide for creating a Service Workbench workspace for Amazon EMR version 6. The default EMR product supplied with EMR version 5, and has older software versions.  For a full list of software versions in EMR 6.5 consult `Amazon EMR release 6.5.0 <https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-650-release.html>`_

+--------+---------+---------+
|        |  EMR 5  |  EMR 6  |
+========+=========+=========+
| Date   | 4/1/19  | 1/20/22 |
+--------+---------+---------+
| EMR    | 5.27.1  | 6.50    |
+--------+---------+---------+
| Spark  | 2.4.4   | 3.1.2   |
+--------+---------+---------+
| Hail   | 0.2.107 | 0.2     |
+--------+---------+---------+
| Hadoop | 2.8.5   | 3.2.1   |
+--------+---------+---------+

----------
Components
----------

There are 3 components making up the Service Workbench EMR workspace type:

AMI Image
    EMR runs on a cluster of EC2 instances comprising one master node and one or more core nodes. Each of these instances runs on the same custom AMI image, which is built as part of the Service Workbench deployment process.  Software versions are built in to the AMI; there are no installations or updates performed when the EMR stack is deployed or when the EC2 instances start.  The AMI image is stored in the Service Workbench main account.

    During the image build, additional packages are downloaded and become part of the machine image.  The primary package installed is :ref:`hail-on-aws-spot-summary`, defined below.

    See the section :ref:`ami-image` for a full description.

CloudFormation Template
    The Service Workbench codebase file ``emr-cluster.cfn.yml`` (`GitHub <https://github.com/awslabs/service-workbench-on-aws/blob/mainline/addons/addon-base-raas/packages/base-raas-cfn-templates/src/templates/service-catalog/emr-cluster.cfn.yml>`_) defines the EMR product.  This template file is launched by CloudFormation when a new EMR workspace is created by the user. 

    See the section :ref:`cloudformation-template` for a full description.
    
.. _hail-on-aws-spot-summary:

Hail on AWS Spot Instances
    This is a package originally created by Harvard DBMI and used within the EMR product.  It is tied to EMR version 5.23 must be cloned and modified in order to upgrade the EMR product.

    See the section :ref:`hail-on-aws-spot` for a full description.