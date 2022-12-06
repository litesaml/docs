---
title: Make AuthnRequest
sidebar_position: 1
---

The initial SAML Message in workflow is an AuthnRequest.  
Service Provider send this message to an Identity Provider in order to initiate authentication.  

In this message, the Service Provider declare :
+ a unique id for the message
+ this entity ID as Issuer
+ the issue instant of the message
+ the destination
+ the protocol in use

This is how you can generate this message :

```php
<?php

$authnRequest = new \LightSaml\Model\Protocol\AuthnRequest();
$authnRequest
    ->setAssertionConsumerServiceURL('https://my.site/acs')
    ->setProtocolBinding(\LightSaml\SamlConstants::BINDING_SAML2_HTTP_POST)
    ->setID(\LightSaml\Helper::generateID())
    ->setIssueInstant(new \DateTime())
    ->setDestination('https://idp.com/login')
    ->setIssuer(new \LightSaml\Model\Assertion\Issuer('https://my.site'))
;
```

The serialisation of this message produce that similar XML :

```xml
<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
    ID="_8dcc6985f6d9f385f0bbd4562ef848ef3ae78d87d7"
    Version="2.0"
    IssueInstant="2022-05-31T22:36:20Z"
    Destination="https://idp.com/login"
    AssertionConsumerServiceURL="https://my.site/acs"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
>
    <saml:Issuer>https://my.site</saml:Issuer>
</samlp:AuthnRequest>
```
