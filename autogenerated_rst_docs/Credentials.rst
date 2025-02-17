Credentials
-----------

The credentials API provides the core functionality for device authentication credentials in Net LineDancer.

Methods in this API return various "credentials objects" (e.g. CredentialConfig) which encapsulate attributes of specific credentials in Net LineDancer. These configuration objects are expressed in JSON format.

.. raw:: html

   <hr>

A *dynamic* ``CredentialConfig`` object is expressed in JSON format seen here:

.. code:: json

   {  
      "priority": 1,
      "addressSet": {  
         "addresses": [  
            "0.0.0.0/0"
         ]
      },
      "name": "LVI",
      "managedNetwork": "Default",
      "flat": false
   }

Here is an example of a *static* ``CredentialConfig`` object. Note the empty ``addresses`` list and ``flat`` value of ``true``:

.. code:: json

   {  
      "priority": 0,
      "addressSet": {  
         "addresses": [ ]
      },
      "name": "Static",
      "managedNetwork": "Default",
      "flat": true
   }

============== ============ =============================================================================================================================================================================
Attribute      Type         Description
============== ============ =============================================================================================================================================================================
priority       Integer      A lower priority number indicates higher placement in the list of credential configurations. These credentials will be tried before others with a higher number.
addressSet     Object       A container for "addresses". Consider this a "fixed" element.
addresses      String Array An array of IP Addresses, IP Wildcards, and IP/CIDR expressions the a "dynamic" ``CredentialConfig`` applies to. *This should be empty for static credential configurations.*
name           String       The name of the ``CredentialConfig`` as seen in the user interface.
managedNetwork String       The name of the network that this configuration applies to. If there are no user-created networks this value should be ``Default``.
flat           Boolean      ``true if`` the ``CredentialConfig`` represents a "static" configuration, ``false`` if it is dynamic.
============== ============ =============================================================================================================================================================================

An example ``CredentialSet`` object is expressed in JSON format seen here:

.. code:: json

   {
      "name": "Lab Credentials #1",
      "username": "lvi",
      "password": "lvi",
      "enableUsername": "lvi",
      "enablePassword": "lvi",
      "roCommunityString": "public",
      "snmpUsername": "v3user",
      "snmpAuthPassword": "v3pass",
      "snmpPrivPassword": "v3priv",
      "priority": 0
   }

================= ======= ===========================================================================================================================================================================================
Attribute         Type    Description
================= ======= ===========================================================================================================================================================================================
name              String  Name of the credential set. In the case of dynamic credentials this is any user defined name; for static credentials the name MUST be an IP address (IPv4 or IPv6).
username          String  The VTY username
password          String  The VTY password
enableUsername    String  The enable username
enablePassword    String  The enable password
roCommunityString String  The SNMP read-only community string
snmpUsername      String  The SNMPv3 username
snmpAuthPassword  String  The SNMPv3 password
snmpAuthPriv      String  The SNMPv3 private password
priority          Integer A lower priority number indicates higher placement in the list of credential sets. These credentials will be tried before others with a higher number. *Inapplicable to static credentials*
================= ======= ===========================================================================================================================================================================================

An example ``PageData`` object is expressed in JSON format seen here:

.. code:: json

   {
       "offset": 0,
       "pageSize": 10,
       "total": 27,
       "credentialSets": [<CredentialSet> objects]
   }

============== ======= =================================================================================================================================================================
Attribute      Type    Description
============== ======= =================================================================================================================================================================
offset         Integer The starting ``offset`` in the results to begin retrieving ``pageSize`` number of ``CredentialSet`` objects.
pageSize       Integer The maximum number of ``CredentialSet`` objects to retrieve in a single method call.
total          Integer This value is set and retrieved from the server when an ``offset`` of zero (0) is passed. This indicates the total number of ``CredentialSet`` objects available.
credentialSets Array   An array of ``CredentialSet`` objects
============== ======= =================================================================================================================================================================

.. raw:: html

   <hr>

Important Note
^^^^^^^^^^^^^^

Maintaining session is important for this API to work correctly. The first invocation will assign to you a session ID by a cookie called ``JSESSIONID``. Use this cookie to maintain session.

After ``commitEdits`` or ``discardEdits`` is invoked, you may discard this cookie.

.. raw:: html

   <hr>

.. _credentialscommitedits:

Credentials.commitEdits
^^^^^^^^^^^^^^^^^^^^^^^

Commits edits made in this session.

Parameters: none
''''''''''''''''

Return: nothing
'''''''''''''''

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.commitEdits",
      "params": {},
      "id": 1
   }

.. raw:: html

   <hr>

.. _credentialsdiscardedits:

Credentials.discardEdits
^^^^^^^^^^^^^^^^^^^^^^^^

Discards edits made in this session.

.. _parameters-none-1:

Parameters: none
''''''''''''''''

.. _return-nothing-1:

Return: nothing
'''''''''''''''

.. _sample-request-json-1:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.discardEdits",
      "params": {},
      "id": 1
   }

.. raw:: html

   <hr>

.. _credentialsgetcredentialconfig:

Credentials.getCredentialConfig
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Retrieves a single credential configuration ``CredentialConfig`` object.

Parameters
''''''''''

========== ====== ====================================================
Parameter  Type   Description
========== ====== ====================================================
network    String Name of an existing network, e.g. "Default"
configName String Name of a dynamic or static credential configuration
========== ====== ====================================================

Return: a ``CredentialConfig`` object
'''''''''''''''''''''''''''''''''''''

.. _sample-request-json-2:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.getCredentialConfig",
      "params": {
          "network": "Default",
          "configName": "LogicVein"
      },
      "id": 1
   }

Sample Response JSON:
'''''''''''''''''''''

.. code:: json

   {  
      "jsonrpc": "2.0",
      "id": 1,
      "result": {  
         "priority": 2,
         "addressSet": {
            "addresses": [
               "0.0.0.0/0"
            ]
         },
         "name": "LogicVein",
         "managedNetwork": "Default",
         "flat": false
      }
   }

.. raw:: html

   <hr>

.. _credentialssavecredentialconfig:

Credentials.saveCredentialConfig
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Save a single credential configuration. This method can be used to save a new configuration, or renaming an existing one. Note: after call this method you must call the ``commitEdits`` or ``discardEdits`` method. If you are making many changes, it is recommended that all changes are should be made before a final call to ``commitEdits``.

.. _parameters-1:

Parameters
''''''''''

================ ====== =======================================================================================================================================================================================================================================
Parameter        Type   Description
================ ====== =======================================================================================================================================================================================================================================
network          String Name of an existing network, e.g. "Default"
oldConfigName    String When *renaming* a credential configuration, this value should be the "old" (original) name of the configuration, and object in the ``credentialConfig`` parameter should contain the new name. This can be ``null`` in all other cases.
credentialConfig Object A ``CredentialConfig`` object
================ ====== =======================================================================================================================================================================================================================================

Return: the updated ``CredentialConfig`` object
'''''''''''''''''''''''''''''''''''''''''''''''

.. _sample-request-json-3:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.saveCredentialConfig",
      "params": {
          "network": "Default",
          "oldConfigName": "LogicVein",
          "credentialConfig": {
              "priority": 1,
              "addressSet": {
                  "addresses": [ "0.0.0.0/0" ]
               },
              "name": "LogicVein2",
              "managedNetwork": "Default",
              "flat": false
         },
      "id": 1
   }

.. raw:: html

   <hr>

.. _credentialsdeletecredentialconfig:

Credentials.deleteCredentialConfig
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Delete a single credential configuration. Note: after call this method you must call the ``commitEdits`` or ``discardEdits`` method. If you are making many changes, it is recommended that all changes are should be made before a final call to ``commitEdits``.

.. _parameters-2:

Parameters
''''''''''

========== ====== ====================================================
Parameter  Type   Description
========== ====== ====================================================
network    String Name of an existing network, e.g. "Default"
configName String Name of a dynamic or static credential configuration
========== ====== ====================================================

.. _return-nothing-2:

Return: nothing
'''''''''''''''

.. _sample-request-json-4:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.deleteCredentialConfig",
      "params": {
          "network": "Default",
          "configName": "LogicVein"
       },
      "id": 1
   }

.. raw:: html

   <hr>

.. _credentialsgetcredentialsets:

Credentials.getCredentialSets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Get the credential sets associated with a specified credential configuration.

.. _parameters-3:

Parameters
''''''''''

========== ======= ================================================================================================
Parameter  Type    Description
========== ======= ================================================================================================
pageData   Object  A credentials page data object (see above)
network    String  Name of an existing network, e.g. "Default"
configName String  Name of a dynamic or static credential configuration
ipOrCidr   String  Can be to search among static ``CredentialSets`` based on IP or IP/CIDR, can be "null"
sortColumn String  Should be "null" for dynamic configurations, or "ipAddress" for static credential configurations
descending Boolean The sort order of the ``CredentialSet`` objects, only applicable to static configurations
========== ======= ================================================================================================

Return: A ``PageData`` object containing a collection of ``CredentialSet`` objects
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

The ``PageData`` object that is returned will contain a property called ``credentialSets``, which is an array of ``CredentialSet`` objects. If the initial ``offset`` that is passed is zero (0), the returned ``PageData`` object will also contain a populated ``total`` attribute, telling you how many total results are available. By incrementing the ``offset``
by ``pageSize`` you can retrieve subsequent pages of results. When ``offset`` + ``pageSize`` is greater than or equal to ``total`` there are no more results available.

.. _sample-request-json-5:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.getCredentialSets",
      "params": {
         "pageData": {
             "offset": 0,
             "pageSize": 10,
             "total": 0,
             "credentialSets": [ ]
          },
         "network": "Default",
         "configName": "LogicVein",
         "ipOrCidr": null,
         "sortColumn": "ipAddress",
         "descending": false
        },
      "id": 1
   }

.. _sample-response-json-1:

Sample Response JSON:
'''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "id": 1,
      "result": {
         "offset": 0,
         "pageSize": 10,
         "total": 2,
         "credentialSets": [
            {
               "name": "Lab Credentials #1",
               "username": "lvi",
               "password": "lvi",
               "enableUsername": "lvi",
               "enablePassword": "lvi",
               "roCommunityString": "public",
               "snmpUsername": "",
               "snmpAuthPassword": "",
               "snmpPrivPassword": "",
               "priority": 0
            },
            {
               "name": "Lab Credentials #2",
               "username": "test",
               "password": "test",
               "enableUsername": "test",
               "enablePassword": "test",
               "roCommunityString": "public",
               "snmpUsername": "test",
               "snmpAuthPassword": "test",
               "snmpPrivPassword": "test",
               "priority": 1
            }
         ]
      }
   }

*Static* credential sets will look identical to *dynamic* credential sets with the exception that the ``name`` property will contain the IP address of the device that the credential set is associated with.

.. raw:: html

   <hr>

.. _credentialssavecredentialsets:

Credentials.saveCredentialSets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Save a collection of credential sets. Note: after call this method you must call the ``commitEdits`` or ``discardEdits`` method. If you are making many changes, it is recommended that all changes are should be made before a final call to ``commitEdits``.

.. _parameters-4:

Parameters
''''''''''

============== ====== ====================================================
Parameter      Type   Description
============== ====== ====================================================
network        String Name of an existing network, e.g. "Default"
configName     String Name of a dynamic or static credential configuration
credentialSets Array  An array of ``CredentialSet`` objects
============== ====== ====================================================

.. _return-nothing-3:

Return: nothing
'''''''''''''''

.. _sample-request-json-6:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.saveCredentialSets",
      "params": {
          "network": "Default",
          "configName": "LogicVein",
          "credentialSets": [
              {
                 "name": "Lab Credentials #1",
                 "username": "newUsername",
                 "password": "lvi",
                 "enableUsername": "newEnable",
                 "enablePassword": "lvi",
                 "roCommunityString": "public",
                 "snmpUsername": "",
                 "snmpAuthPassword": "",
                 "snmpPrivPassword": "",
                 "priority": 0
              }
          ]
       },
      "id": 1
   }

.. raw:: html

   <hr>

.. _credentialsdeletecredentialsets:

Credentials.deleteCredentialSets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Delete a collection of credential sets. Note: after call this method you must call the ``commitEdits`` or ``discardEdits`` method. If you are making many changes, it is recommended that all changes are should be made before a final call to ``commitEdits``.

.. _parameters-5:

Parameters
''''''''''

============== ====== ====================================================
Parameter      Type   Description
============== ====== ====================================================
network        String Name of an existing network, e.g. "Default"
configName     String Name of a dynamic or static credential configuration
credentialSets Array  An array of ``CredentialSet`` objects
============== ====== ====================================================

.. _return-nothing-4:

Return: nothing
'''''''''''''''

.. _sample-request-json-7:

Sample Request JSON:
''''''''''''''''''''

.. code:: json

   {
      "jsonrpc": "2.0",
      "method": "Credentials.deleteCredentialSets",
      "params": {
         "network": "Default",
         "configName": "LogicVein",
         "credentialSets": [
              {
                 "name": "Old Credentials #1",
                 "username": "lvi",
                 "password": "lvi",
                 "enableUsername": "newEnable",
                 "enablePassword": "lvi",
                 "roCommunityString": "public",
                 "snmpUsername": "",
                 "snmpAuthPassword": "",
                 "snmpPrivPassword": "",
                 "priority": 0
              },
              {
                 "name": "Old Credentials #2",
                 "username": "test",
                 "password": "test",
                 "enableUsername": "test",
                 "enablePassword": "test",
                 "roCommunityString": "public",
                 "snmpUsername": "",
                 "snmpAuthPassword": "",
                 "snmpPrivPassword": "",
                 "priority": 1
              }
          ]
       }
      "id": 1
   }
