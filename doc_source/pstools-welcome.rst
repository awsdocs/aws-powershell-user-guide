.. _pstools-welcome:

#######################
What are the |TWPlong|?
#######################

The |TWPlong| are a set of PowerShell cmdlets that are built on the functionality exposed by the
|sdk-net|. The |TWPlong| enable you to script operations on your AWS resources from the PowerShell
command line. Although the cmdlets are implemented using the service clients and methods from the
SDK, the cmdlets provide an idiomatic PowerShell experience for specifying parameters and handling
results. For example, the cmdlets for the |TWP| support PowerShell pipelining |mdash| that is, you
can pipeline PowerShell objects both into and out of the cmdlets.

The |TWPlong| are flexible in how they enable you to handle credentials including support for the
|IAMlong| (|IAM|) infrastructure; you can use the tools with |IAM| user credentials, temporary
security tokens, and |IAM| roles.

The |TWPlong| support the same set of services and regions as supported by the SDK.


How to Use this Guide
=====================

The guide is divided into the following major sections:

:ref:`pstools-getting-set-up`
    This section explains how to install the |TWPlong|. It also covers how to sign up for AWS if
    you don't already have an account. (An AWS account is required in order to use the |TWP|.)

:ref:`pstools-getting-started`
    This section describes the fundamentals of using the tools, such as specifying credentials and
    regions, finding cmdlets for a particular service, and using aliases for cmdlets.

:ref:`pstools-using`
    This section includes information about using the |TWPlong| to perform common AWS tasks.



