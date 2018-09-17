.. _pstools-lambda:

######################
|LAMlong| and |TWPALL|
######################

With the `AWSLambdaPSCore <https://www.powershellgallery.com/packages/AWSLambdaPSCore/1.0.0.2>`_ module, you can develop |LAMlong| functions in PowerShell Core 6.0 using the .NET Core 2.1 runtime. PowerShell developers can manage AWS resources and write automation scripts in the PowerShell environment by using |LAM|. PowerShell support in |LAM| lets you run PowerShell scripts or functions in response to any Lambda event, such as an |S3| event or |CWlong| scheduled event. The AWSLambdaPSCore module is a separate AWS module for PowerShell; it is not part of the |TWPALLlong|, nor does installing the AWSLambdaPSCore module install the |TWPALL|.

After you install the AWSLambdaPSCore module, you can use any available PowerShell cmdlets |mdash| or develop your own |mdash| to author serverless functions. The AWS Lambda Tools for PowerShell module includes project templates for PowerShell-based serverless applications, and tools to publish projects to AWS.

AWSLambdaPSCore module support is available in all regions that support |LAM|. For more information about supported regions, see the `AWS region table <https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/>`_.

Prerequisites
=============

The following steps are required before you can install and use the AWSLambdaPSCore module. For more detail about these steps, see `Setting Up a PowerShell Development Environment <https://docs.aws.amazon.com/lambda/latest/dg/lambda-powershell-setup-dev-environment.html>`_ in the |LAMlong| Developer Guide.

* **Install the correct release of PowerShell** – Lambda's support for PowerShell is based on the cross-platform PowerShell Core 6.0 release. You can develop PowerShell |LAM| functions on Windows, Linux, or Mac. If you don’t have this release of PowerShell installed, instructions are available on the `Microsoft PowerShell documentation website <https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell?view=powershell-6>`_.

* **Install the .NET Core 2.1 SDK** – Because PowerShell Core is based on .NET Core, the |LAM| support for PowerShell uses the same .NET Core 2.1 |LAM| runtime for both .NET Core and PowerShell |LAM| functions. The |LAM| PowerShell publishing cmdlets use the .NET Core 2.1 SDK to create the |LAM| deployment package. The .NET Core 2.1 SDK is available from the `Microsoft Download Center <https://www.microsoft.com/net/download>`_. Be sure to install the SDK, not the Runtime.

Install the AWSLambdaPSCore Module
==================================

After completing the prerequisites, you are ready to install the AWSLambdaPSCore module. Run the following command in a PowerShell Core session.

.. code-block:: none

    PS> Install-Module AWSLambdaPSCore -Scope CurrentUser

You are ready to start developing |LAM| functions in PowerShell. For more information about how to get started, see `Programming Model for Authoring Lambda Functions in PowerShell <https://docs.aws.amazon.com/lambda/latest/dg/powershell-programming-model.html>`_ in the |LAMlong| Developer Guide.

See Also
========

* `Announcing Lambda Support for PowerShell Core on the AWS Developer Blog <https://aws.amazon.com/blogs/developer/announcing-lambda-support-for-powershell-core/>`_

* `AWSLambdaPSCore module on the PowerShell Gallery website <https://www.powershellgallery.com/packages/AWSLambdaPSCore/1.0.0.2>`_

* `Setting Up a PowerShell Development Environment <https://docs.aws.amazon.com/lambda/latest/dg/lambda-powershell-setup-dev-environment.html>`_

* `AWS Lambda Tools for Powershell on GitHub <https://github.com/aws/aws-lambda-dotnet/tree/master/PowerShell>`_

* `AWS Lambda Console <https://console.aws.amazon.com/lambda/home>`_

