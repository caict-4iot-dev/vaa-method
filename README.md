# VAA DID Method Specification

Version 1.0.0

## About

This [DID method specification](https://w3c-ccg.github.io/did-spec/#specific-did-method-schemes)  conforms to the requirements in the DID specification currently published by the W3C Credentials Community Group. For more information about DIDs and DID method specifications, please see the [DID Primer](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/topics-and-advance-readings/did-primer.md) and [DID Spec](https://w3c.github.io/did-core/).

The following DID Method will be registered in the [DID Specification Registries](https://w3c.github.io/did-spec-registries/#did-methods).

## Abstract

Blockchain Identifier Infrastructure (BIF) is a permissioned public blockchain aiming for creating a distributed trust management framework typical for internet ID service, and the BIF blockchain (http://bidspace.cn/) is governed by China Academy of Information and Communications Technology (CAICT). CAICT is also the official issuing agency with Issuing Agency Code (IAC)——"VAA", given by ISO/IEC 15459. The IAC indicates an authorized qualification of distributing identifiers with own allocation rules.

As the "VAA" means a self-defined rule for internet identifiers, these letters could be literal used as a DID method name conformant with W3C DID specification. The VAA method is created not only to define a random identifier generation rules, but ultimately to create smart contracts, which support different identifiers format.



## Status of  This Document

This document was published as an Editor's Draft.

Publication as an Editor's Draft does not imply endorsement by the W3C Membership. This is a draft document and may be updated, replaced or obsoleted by other documents at any time. It is inappropriate to cite this document as other than a work in progress.

## VAA DID Method

- The namestring that shall identify this DID method is : vaa
- A DID that uses this method MUST begin with the following prefix: did:vaa. Per the DID specification, this string MUST be in lowercase. The remainder of the DID, after the prefix, is the NSI specified below.

### Namespace Specific Identifier (NSI)

Set random generation identifiers as a **default contract** , method-specific-id should be encoded as follow:

1) Generate random public key through Elliptic-curve cryptography (ECC) algorithm 

2) Hash the public key and take the first 20 digits

3) Use base58 encode the first 20 digits to obtain the method-specific-id

    vaa-did = did:vaa:method-specific-id
    vaa-specific-id = 27*31(base58char)
    base58char = "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9" / "A" / "B" / "C"
                 / "D" / "E" / "F" / "G" / "H" / "J" / "K" / "L" / "M" / "N" / "P" / "Q"
                 / "R" / "S" / "T" / "U" / "V" / "W" / "X" / "Y" / "Z" / "a" / "b" / "c"
                 / "d" / "e" / "f" / "g" / "h" / "i" / "j" / "k" / "m" / "n" / "o" / "p"
                 / "q" / "r" / "s" / "t" / "u" / "v" / "w" / "x" / "y" / "z"



Examples:

```
did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB
```



Set smart contracts for different usecases:

1) Name the encoding rule as IDcontract1

2) Reserve an identifier allocation list for IDcontract1 in each of the BIF Blockchain nodes

3) Generate an identifier compliance with the encoding rule for new  identifier application, not conflict with existed numbers in the identifier allocation list



Examples:

```
did:vaa:IDcontract1:86.100.1234/abcdfe.123
```



## DID Document

    {
        "@context": "https://www.w3.org/2019/did/v1",
        "id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
        "controllor":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
        "verificationMethod": [{
        	"id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#key1",
        	"type": "RsaVerificationKey2018",
        	"controller": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
      		"publicKeyPem": "-----BEGIN PUBLIC KEY...END PUBLIC KEY-----\r\n"
      ]},
         "service": [{
             "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#resolver",
             "type": "DIDResolve",
             "serviceEndpoint": "http://bidspace.cn/"
        ]}
         "created":  "20200101235959",
         "updated": "20200101235959"
    }



## VAA DID Method Specification        

### Create

For default contract rule, a DID document is created by performing an HTTP POST containing following elements.                            

To create a DID document, you must submit a transaction that looks like this:


    POST /vaa  HTTP/1.1
    Content-Type: application/ld+json
    application/json, text/plain, */*
    Accept-Encoding: gzip, deflate
    {
    "id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
    "opration":"create",
    "document":  {
        "@context": "https://w3id.org/did/v1",
        "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
        "publicKey": [
         {
                "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#key1",
                "type": "RsaVerificationKey2018",
                "publicKeyPem": "--BEGIN....END---"
        }],
        "service": [
        {
                "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#resolver",
                "type": "DIDResolve",
                "serviceEndpoint": "http://bidspace.cn/"
        }],
        "created":  "20200101235959",
        "updated": "20200101235959"
    }
    }

### Read 

A DID Document can be read by performing an HTTP GET containing following elements.                                  

To query a DID document, you must submit a transaction that looks like this:

GET /did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB  HTTP/1.1
Content-Type: application/ld+json
Accept: application/ld+json, application/json, text/plain, */*
Accept-Encoding: gzip, deflate

To read an identifier, look it up from the blockchain, and then directly obtain the DID document, for example:

```
{
"responseCode":0,
"id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
"document":  {
    "@context": "https://w3id.org/did/v1",
    "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
    "publicKey": [
     {
            "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#key1",
            "type": "RsaVerificationKey2018",
            "publicKeyPem": "--BEGIN....END---"
    }],
    "service": [
    {
            "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#resolver",
            "type": "DIDResolve",
            "serviceEndpoint": "http://bidspace.cn/"
    }],
    "created":  "20200101235959",
    "updated": "20200101235959"
}
}
```



### Update

A DID document can be replaced by performing an HTTP POST containing following elements.                     
To update a DID document, you must submit a transaction that looks like this:
 POST /vaa HTTP/1.1
    Content-Type: application/ld+json
    application/json, text/plain, */*
    Accept-Encoding: gzip, deflate

    {
    "id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
    "opration":"update",
    "document":  {
        "@context": "https://w3id.org/did/v1",
        "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
        "publicKey": [
         {
                "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#keys",
                "type": "RsaVerificationKey2018",
                "publicKeyPem": "--BEGIN....END---"
        }],
        "service": [
        {
                "id":"did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB#resolver",
                "type": "DIDResolve",
                "serviceEndpoint": "http://bidspace.cn/"
        }],
        "created":  "20200101235959",
        "updated": "20200101235959"
    	}
    }

### Delete（Revoke）

Deletion means invalidation of the existing DID. An invalid DID has its revoked date time set;

    "revoked": "2020-01-01T15:09:03.3712946+13:00"

A DID document can be revoked by performing an HTTP POST.             
To revoke the document of the DID, the owner of the DID should send a request that looks like this:

POST /vaa  HTTP/1.1
    Content-Type: application/ld+json
    application/json, text/plain, */*
    Accept-Encoding: gzip, deflate

     {
    	"id": "did:vaa:gxAcE6upFw6DNbG3rfngM3VxCNB",
    	"operation": " delete"
    }



## Security and Privacy Considerations

- The information related to the user's privacy is not linked, or on the authority side, the statement submitted by the authority is used to prove the user's proprietary attribute, so as to better protect the user's privacy from being replaced.
- It can be proved that the private key owned by the DID only exists on the user's device and will not be known to any third party.
- DID documents use signature technology to prevent malicious error correction.
