.. _pstools-ec2-keypairs:

###################
Creating a Key Pair
###################

The following example uses the :code:`New-EC2KeyPair` cmdlet to create a key pair. The returned
object is stored in the PowerShell variable :code:`$myPSKeyPair`

.. code-block:: none

    PS C:\> $myPSKeyPair = New-EC2KeyPair -KeyName myPSKeyPair

Pipe the key pair object into the :code:`Get-Member` cmdlet to view the object's members.

.. code-block:: none

    PS C:\> $myPSKeyPair | Get-Member
    
         TypeName: Amazon.EC2.Model.KeyPair
    
      Name                MemberType   Definition
      ----                ----------   ----------
      Equals              Method       bool Equals(System.Object obj)
      GetHashCode         Method       int GetHashCode()
      GetType             Method       type GetType()
      ToString            Method       string ToString()
      KeyFingerprint      Property     System.String KeyFingerprint {get;set;}
      KeyMaterial         Property     System.String KeyMaterial {get;set;}
      KeyName             Property     System.String KeyName {get;set;}

Pipe the key pair object into the :code:`Format-List` cmdlet to view values of the :code:`KeyName`,
:code:`KeyFingerprint`, and :code:`KeyMaterial` members. (The output has been truncated for
readability.)

.. code-block:: none

    PS C:\> $myPSKeyPair | Format-List KeyName, KeyFingerprint, KeyMaterial
    
      KeyName        : myPSKeyPair
      KeyFingerprint : 09:06:70:8e:26:b6:e7:ef:8f:fe:4a:1d:bc:9c:6a:63:11:ac:ad:3c
      KeyMaterial    : ----BEGIN RSA PRIVATE KEY----
                       MIIEogIBAAKCAQEAkK+ANYUS9c7niNjYfaCn6KYj/D0I6djnFoQE...
                       Mz6btoxPcE7EMeH1wySUp8nouAS9xbl9l7+VkD74bN9KmNcPa/Mu...
                       Zyn4vVe0Q5il/MpkrRogHqOB0rigeTeV5Yc3lvO0RFFPu0Kz4kcm...
                       w3Jg8dKsWn0plOpX7V3sRC02KgJIbejQUvBFGi5OQK9bm4tXBIeC...
                       daxKIAQMtDUdmBDrhR1/YMv8itFe5DiLLbq7Ga+FDcS85NstBa3h...
                       iuskGkcvgWkcFQkLmRHRoDpPb+OdFsZtjHZDpMVFmA9tT8EdbkEF...
                       3SrNeqZPsxJJIxOodb3CxLJpg75JU5kyWnb0+sDNVHoJiZCULCr0...
                       GGlLfEgB95KjGIk7zEv2Q7K6s+DHclrDeMZWa7KFNRZuCuX7jssC...
                       xO98abxMr3o3TNU6p1ZYRJEQ0oJr0W+kc+/8SWb8NIwfLtwhmJEy...
                       1BX9X8WFX/A8VLHrT1elrKmLkNECgYEAwltkV1pOJAFhz9p7ZFEv...
                       vvVsPaF0Ev9bk9pqhx269PB5Ox2KokwCagDMMaYvasWobuLmNu/1...
                       lmwRx7KTeQ7W1J3OLgxHA1QNMkip9c4Tb3q9vVc3t/fPf8vwfJ8C...
                       63g6N6rk2FkHZX1E62BgbewUd3eZOS05Ip4VUdvtGcuc8/qa+e5C...
                       KXgyt9nl64pMv+VaXfXkZhdLAdY0Khc9TGB9++VMSG5TrD15YJId...
                       gYALEI7m1jJKpHWAEs0hiemw5VmKyIZpzGstSJsFStERlAjiETDH...
                       YAtnI4J8dRyP9I7BOVOn3wNfIjk85gi1/0Oc+j8S65giLAfndWGR...
                       9R9wIkm5BMUcSRRcDy0yuwKBgEbkOnGGSD0ah4HkvrUkepIbUDTD...
                       AnEBM1cXI5UT7BfKInpUihZi59QhgdK/hkOSmWhlZGWikJ5VizBf...
                       drkBr/vTKVRMTi3lVFB7KkIV1xJxC5E/BZ+YdZEpWoCZAoGAC/Cd...
                       TTld5N6opgOXAcQJwzqoGa9ZMwc5Q9f4bfRc67emkw0ZAAwSsvWR...
                       x3O2duuy7/smTwWwskEWRK5IrUxoMv/VVYaqdzcOajwieNrblr7c...
                       -----END RSA PRIVATE KEY-----

The :code:`KeyMaterial` member stores the private key for the key pair. The public key is stored in
AWS. You can't retrieve the public key from AWS, but you can verify the public key by comparing the
:code:`KeyFingerprint` for the private key to that returned from AWS for the public key.


.. _get-ec2keypair:

Viewing the Fingerprint of Your Key Pair
----------------------------------------

You can use the :code:`Get-EC2KeyPair` cmdlet to view the fingerprint for your key pair.

.. code-block:: none

    PS C:\> Get-EC2KeyPair -KeyName myPSKeyPair | format-list KeyName, KeyFingerprint
    
      KeyName        : myPSKeyPair
      KeyFingerprint : 09:06:70:8e:26:b6:e7:ef:8f:fe:4a:1d:bc:9c:6a:63:11:ac:ad:3c


.. _store-ec2keypair:

Storing Your Private Key
------------------------

To store the private key to a file, pipe the :code:`KeyFingerMaterial` member to the
:code:`Out-File` cmdlet.

.. code-block:: none

    PS C:\> $myPSKeyPair.KeyMaterial | Out-File -Encoding ascii myPSKeyPair.pem

You must specify :code:`-Encoding ascii` when writing the private key to a file. Otherwise, tools
such as :code:`openssl` may not be able to read the file correctly. You can verify that the format
of the resulting file is correct by using a command such as the following:

.. code-block:: none

    openssl rsa -check < myPSKeyPair.pem

(The :code:`openssl` tool is not included with the |TWPlong| or the AWS SDK for .NET.)


.. _remove-ec2keypair:

Removing Your Key Pair
----------------------

You'll need your key pair to launch and connect to an instance. When you have finished using a key
pair, you can remove it. To remove the public key from AWS, use the :code:`Remove-EC2KeyPair`
cmdlet. When prompted, press :kbd:`Enter` to remove the key pair.

.. code-block:: none

    PS C:\> Remove-EC2KeyPair -KeyName myPSKeyPair
      
    Remove-EC2KeyPair
    Are you sure you want to remove keypair 'myPSKeyPair'?
    [Y] Yes  [N]  [S] Suspend  [?] Help (default is "Y"):

The variable, :code:`$myPSKeyPair`, still exists in the current PowerShell session and still
contains the key pair information. The :file:`myPSKeyPair.pem` file also exists. However, the
private key is no longer valid because the public key for the key pair is no longer stored in AWS.
