.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _ecr-pushimage:

############################################
Amazon Elastic Container Registry Push Image
############################################

.. meta::
   :description: AWS Tools for Visual Studio Team Services (VSTS) Task Reference
   :keywords: extensions, tasks

Synopsis
========

Pushes a Docker image identified by name, with optional tag, or image ID to the Elastic Container Registry (ECR).

Description
===========

This task pushes a Docker image to the Elastic Container Registry. The image to push can be identified using its
image ID or by name, with optional tag suffix. The task handles the work of appropriately tagging the image as required
by ECR and also the login process to your registry prior to executing the Docker Push command.

Parameters
==========

You can set the following parameters for the task. Required parameters are noted by an asterisk (*).
Other parameters are optional.

Display name*
-------------

    The default name of the task instance, which can be modified: Push Image

AWS Credentials
---------------

    Specifies the AWS credentials to be used by the task in the build agent environment.

    You can specify credentials using a service endpoint (of type AWS) in the task configuration or you can leave unspecified. If
    unspecified the task will attempt to obtain credentials from the following sources in order:

    * From task variables named *AWS.AccessKeyID*, *AWS.SecretAccessKey* and optionally *AWS.SessionToken*.
    * From credentials set in environment variables in the build agent process. When using environment variables in the
      build agent process you may use the standard AWS environment variables: *AWS_ACCESS_KEY_ID*, *AWS_SECRET_ACCESS_KEY* and
      optionally *AWS_SESSION_TOKEN*.
    * If the build agent is running on an Amazon EC2 instance, from the instance metadata associated with the EC2 instance. For
      credentials to be available from EC2 instance metadata the instance must have been started with an instance profile referencing
      a role granting permissions to the task to make calls to AWS on your behalf. See IAMRolesForEC2_ for more information.

AWS Region
----------

    The AWS region code (us-east-1, us-west-2 etc) of the region containing the AWS resource(s) the task will use or create. For more
    information, see :aws-gr:`Regions and Endpoints <rande>` in the |AWS-gr|.

    If a region is not specified in the task configuration the task will attempt to obtain the region to be used using the standard
    AWS environment variable *AWS_REGION* in the build agent process's environment. Tasks running in build agents hosted on Amazon EC2
    instances (Windows or Linux) will also attempt to obtain the region using the instance metadata associated with the EC2 instance
    if no region is configured on the task or set in the environment variable.

    **Note:** The regions listed in the picker are those known at the time this software was released. New regions that are not listed
    may still be used by entering the *region code* of the region (for example *us_west_2*).

Image Identity*
---------------

    How the image to be pushed is identified. You can select from either the image ID or the image name. If image name
    is selected a tag can also be specified.

Source Image Name
-----------------

    The name of the image to push. Required if *Image Identity* is set to *Image name with optional tag*.

Source Image Tag
----------------

    Optional tag that can be suffixed to the image name. If a tag is not specified, 'latest' is assumed.

Source Image ID
---------------

    The ID of the image to push. Required if *Image Identity* is set to *Image ID*.

Target Repository Name*
-----------------------

    The name of the repository to which the image will be pushed.

Target Repository Tag
---------------------

    Optional tag for the new image in the repository. If not specified, ECR will assume 'latest'.

Create repository if it does not exist
--------------------------------------

    If checked, the task will check to see if the repository exists and if it does not, will attempt to create it.

Image Tag Output Variable
-------------------------

    The name of a build variable that will be created or updated with the pushed image reference.
    The image tag will be of the form *aws_account_id.dkr.ecr.region.amazonaws.com/imagename*,
    where **imagename** is in the format *repositoryname[:tag]*

Task Permissions
================

This task requires permissions to call the following AWS service APIs (depending on selected task options, not all APIs may be used):

  * ecr:DescribeRepositories
  * ecr:CreateRepository
  * ecr:GetAuthorizationToken
