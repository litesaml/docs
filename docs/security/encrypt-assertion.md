---
title: Encrypt Assertion
sidebar_position: 4
---

Light Saml supports SAML Assertion encryption. First you need normally to create the ``Assertion`` with all the data you need. Then
create a new instance of ``EncryptedAssertionWriter`` and call it's ``encrypt`` method with the created assertion object and
certificate of the recipient. Finally, assign that encryption writer to the SAML Response.

```php
<?php
$assertion = new Assertion();
// fill it up with data...
$certificate = X509Certificate::fromFile('/path/to/saml.crt');
$encryptedAssertion = new EncryptedAssertionWriter();
$encryptedAssertion->encrypt($assertion, KeyHelper::createPublicKey($certificate));
$response = new Response();
$response->addEncryptedAssertion($encryptedAssertion);
$context = new SerializationContext();
$response->serialize($context->getDocument(), $context);
```

**Note**: The SAML protocol also specifies that Assertion should be signed before encryption, while during receiving the Assertion
procedure is reversed - you first decrypt it, and then verify its signature.
