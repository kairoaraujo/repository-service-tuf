###############
RSTUF Processes
###############

RSTUF and TUF key management
############################

RSTUF deployment will require some keys as defined by The Update Framework
(TUF).

The root key(s)
`should be stored secured offline <https://theupdateframework.github.io/specification/latest/#key-management-and-migration>`_.

The online key will be provided during the deployment configuration and used in
a supported
`Key Vault Service <https://repository-service-tuf.readthedocs.io/en/latest/guide/repository-service-tuf-worker/Docker_README.html#required-rstuf-keyvault-backend>`_
by the RSTUF Worker service.

Optionally, RSTUF CLI provides a feature to generate keys.
See :ref:`Key Management <guide/repository-service-tuf-cli/index:Key Management (``key\`\`)>`

The deployment will require the online key to deploy and start RSTUF Worker
service(s).

The Update Framework requires all these keys (root and online key) to generate
the initial metadata. This process is the ceremony of signing the TUF metadata.

TUF Metadata Signing Ceremony
#############################

This process generates the initial metadata and defines some settings of your
TUF service (such as metadata expiration, root signing threshold, etc.).

To make this process easier,
the :ref:`guide/repository-service-tuf-cli/index:Repository Service for TUF CLI`
provides an interactive guided process to perform the
:ref:`Ceremony <guide/repository-service-tuf-cli/index:Ceremony (``ceremony\`\`)>`.

We have a video to show this process.

   .. raw:: html

      <div style="position: relative; padding-bottom: 56.25%; height: 0; margin-bottom: 2em; overflow: hidden; max-width: 100%; height: auto;">
         <iframe src="https://www.youtube.com/embed/j18NvkNfs2A" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
      </div>

Metadata Rotation
#################

.. code:: python

   3890814e661d94084212972ba0c0a5c8ce27': <tuf.api.metadata.Key object at 0x10fd8e020>}
   >>> root.signed.roles
   {'root': <tuf.api.metadata.Role object at 0x1117430a0>, 'snapshot': <tuf.api.metadata.Role object at 0x111743fa0>, 'targets': <tuf.api.metadata.Role object at 0x111742dd0>, 'timestamp': <tuf.api.metadata.Role object at 0x111742d40>}
   >>> root.signed.roles['root']
   <tuf.api.metadata.Role object at 0x1117430a0>
   >>> root.signed.roles['root'].keyids
   ['1cebe343e35f0213f6136758e6c3a8f8e1f9eeb7e47a07d5cb336462ed31dcb7', '800dfb5a1982b82b7893e58035e19f414f553fc08cbb1130cfbae302a7b7fee5', '7feb22ed6a1139913ebdbfac85513890814e661d94084212972ba0c0a5c8ce27']
   >>> root.signed.roles['root'].thresold = 2
   >>> root.signed.version
   1
   >>> root.signed.version = 2
   >>> root.signatures.clear()
   >>> root.sign(key_jj_signer, append=True)
   <securesystemslib.signer._signature.Signature object at 0x10fd8eaa0>
   >>> root.signatures
   {'1cebe343e35f0213f6136758e6c3a8f8e1f9eeb7e47a07d5cb336462ed31dcb7': <securesystemslib.signer._signature.Signature object at 0x10fd8eaa0>}
   >>> root.sign(key_new_signer, append=True)
   <securesystemslib.signer._signature.Signature object at 0x10fcc4970>
   >>> root.signatures
   {'1cebe343e35f0213f6136758e6c3a8f8e1f9eeb7e47a07d5cb336462ed31dcb7': <securesystemslib.signer._signature.Signature object at 0x10fd8eaa0>, '7feb22ed6a1139913ebdbfac85513890814e661d94084212972ba0c0a5c8ce27': <securesystemslib.signer._signature.Signature object at 0x10fcc4970>}
   >>> root.signatures.clear()
   >>> import datetime
   >>> datetime.datetime.now()
   datetime.datetime(2023, 4, 18, 8, 50, 13, 733526)
   >>> datetime.datetime.now().replace(microsecond=0) + datetime.timedelta(days=365)
   datetime.datetime(2024, 4, 17, 8, 51, 30)
   >>> root.signed.expires = datetime.datetime.now().replace(microsecond=0) + datetime.timedelta(days=365)
   >>> root.signatures.clear()
   >>> root.sign(key_jj_signer, append=True)
   <securesystemslib.signer._signature.Signature object at 0x110c5ac50>
   >>> root.sign(key_new_signer, append=True)
   <securesystemslib.signer._signature.Signature object at 0x10fd8f880>
   >>> root.to_file('metadata/2.root.json')
   >>> quit()
