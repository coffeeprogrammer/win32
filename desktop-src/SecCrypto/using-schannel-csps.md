---
Description: The SSL protocol engine (Schannel) uses a cryptographic service provider (CSP) when performing cryptographic operations. Cryptographic applications can call CryptAcquireContext using the PROV\_RSA\_SCHANNEL and PROV\_DH\_SCHANNEL providers.
ms.assetid: ad1eabf1-23bc-4d23-8f8b-13f0bda95120
title: Using Schannel CSPs
ms.topic: article
ms.date: 05/31/2018
---

# Using Schannel CSPs

The SSL protocol engine ([*Schannel*](https://msdn.microsoft.com/library/ms721625(v=VS.85).aspx)) uses a [*cryptographic service provider*](https://msdn.microsoft.com/library/ms721572(v=VS.85).aspx) (CSP) when performing cryptographic operations. Cryptographic applications can call [**CryptAcquireContext**](/windows/desktop/api/Wincrypt/nf-wincrypt-cryptacquirecontexta) using the PROV\_RSA\_SCHANNEL and PROV\_DH\_SCHANNEL providers.

This section defines the RSA and Diffie-Hellman Schannel [*CSP types*](https://msdn.microsoft.com/library/ms721572(v=VS.85).aspx) and describes the functionality that a CSP must support to be compatible with Schannel.dll, the cryptographic protocol engine. A protocol engine is a program that establishes a secure communications channel between a client and server application.

Applications should not attempt to use information in this documentation to use PROV\_RSA\_SCHANNEL or PROV\_DH\_SCHANNEL directly. Rather, this documentation explains how CSP developers and vendors must write Schannel CSPs that are compatible with Microsoft Schannel providers.

This documentation is intended to help CSP developers implement compatible RSA or Diffie-Hellman Schannel CSPs. Developers are assumed to be familiar with the [*Secure Socket Layer*](https://msdn.microsoft.com/library/ms721625(v=VS.85).aspx) (SSL) protocol version 3.0, [*public key*](https://msdn.microsoft.com/library/ms721603(v=VS.85).aspx) cryptography, [*digital certificates*](https://msdn.microsoft.com/library/ms721573(v=VS.85).aspx), and the CryptoAPI function set. Developers new to these topics are advised to read the SSL Protocol 3.0 specification and the [Cryptography Essentials](cryptography-essentials.md) documentation in this SDK. In addition, RSA and Diffie-Hellman CSP developers must know [*Transport Layer Security*](https://msdn.microsoft.com/library/ms721627(v=VS.85).aspx) (TLS) protocol specifications along with the relevant RSA and [*Diffie-Hellman algorithms*](https://msdn.microsoft.com/library/ms721573(v=VS.85).aspx).

For an example used by a Microsoft protocol engine, see [Creating the Master Key](creating-the-master-key.md). The calls to cryptography functions in this example result in calls to CP functions that a CSP must implement. To write a compatible CSP, a developer must understand the SSL 3.0 specification and combine that knowledge with an understanding of the protocol engine code similar to that used in this example.

Because use of the Private Communications Technology protocol is expected to be minimal in the future, developers of new CSPs need not support this protocol. The Schannel protocol engine supports it strictly for backward compatibility.

??

??



