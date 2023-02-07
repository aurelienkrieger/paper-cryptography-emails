
# Abstract

This article offers an overview about how to setup and secure an email account in order to establish a secure and private conversation. Please note that this is an introduction guide, it is still a work-in-progress and anyone who needs reliable security would need to undertake further research. Also note this article highlights how to secure a connection (hence to protect the data), not to make it anonymous which is another topic.

_last update : 26.01.2023_

# 1. Threats
 
The use of email almost always comes in two forms:

* Email read, written and sent via the _browser_ (webmail), or
    
* Email read, written and sent using an _email program_, locally on your computer.

## At rest

_"Data at rest means data that is housed physically on computer data storage in any digital form (e.g. cloud storage, file hosting services, databases, data warehouses, spreadsheets, archives, tapes, off-site or cloud backups, mobile devices etc.). This type of data is subject to threats from hackers and other malicious threats to gain access to the data digitally or physical theft of the data storage media."_ [^1]

This is defined in opposition to data that is moving across a network. 

When using webmail, your emails are stored on the servers of your email provider, but also on the servers of your recipient's email provider, which brings several issues:
 
Email providers can scan your emails, looking for relevant data to sell to data brokers. Data brokers then collect this personal data (such as your income, ethnicity, political beliefs, or geolocation data) and sell it to third parties, that then target you with ads.

Email providers may also be requested by authorities to provide them with this personal data. So there is always a risk of them yielding to pressures from political, economic and law enforcement interests of the country to which they are legally subject. Remember that Gmail for instance is subject to US laws.

Finally, servers from email providers can be hijacked or attacked by hackers.

## In transit

An email transiting over the Internet does not directly move from origin to destination. Instead, it is routed across different networks, passed on from computer to computer until it finally reaches its destination. This exposes the data to "man-in-the-middle" attacks such as eavesdropping or tampering, when someone sniffs networks in order to retrieve or tamper the information that is being communicated between two parties, without them noticing.

HTTP is the protocol used for transferring data (such as websites or multimedia files) between clients and servers. Whether you use webmail or an email program you should always be sure to use encryption for the entire session, from login to logout. See section [HTTPS](#https).

# 2. Solutions

## Cryptography

_"Cryptography (from Ancient Greek: κρυπτός, romanized: kryptós "hidden, secret"; and γράφειν graphein, "to write", or -λογία -logia, "study"), is the practice and study of techniques for secure communication in the presence of adversarial behavior. More generally, cryptography is about constructing and analyzing protocols that prevent third parties or the public from reading private messages."_ [^2]

There are two main types of cryptosystems: symmetric and asymmetric.

In symmetric systems, data is encrypted and decrypted with a secret key known to both sender and recipient. It is efficient in terms of computation, but since both parties hold a common secret key it needs to be shared in a secure manner.

Asymmetric systems use a "public key" to encrypt a message and a related "private key" to decrypt it. The advantage of asymmetric systems is that the public key can be freely published, allowing parties to establish secure communication without having a shared secret key.

In practice, asymmetric systems are used to first exchange a secret key, and then secure communication proceeds via a more efficient symmetric system using that key, like for HTTPS.

## HTTPS

_"Transport Layer Security (TLS) encrypts data sent over a computer network to ensure that eavesdroppers and hackers are unable to see what you transmit which is particularly useful for private and sensitive information such as passwords, credit card numbers, and personal correspondence."_  [^3]

Hypertext Transfer Protocol Secure (HTTPS) is an extension of HTTP which uses TLS for secure communication over a computer network such as the Internet. Therefore HTTPS is designed to protect data in transit.

TLS uses asymmetric encryption to first establish the identity of one or both parties. Secondly, it uses asymmetric encryption to exchange a key to a symmetric cipher. So asymmetric is only used during the initial setup of communication. Symmetric encryption which is used through the rest is faster and more efficient with large amounts of data transfer. The combination of symmetric and asymmetric cryptography provides a good compromise between performance and security.

 _"It’s important to note that the administrators at providers like Hotmail or Google, that host, receive or forward your email can read your email even if you are using secure connections. It is also worth nothing that the cryptographic keys protecting a TLS connection can be deliberately disclosed by site operators, or copied without their permission, breaching the confidentiality of that connection. It is also possible for a Certificate Authority to be corrupted or compromised so that it creates false certificates for keys held by eavesdroppers, making it much easier for a Man In The Middle Attack on connections using TLS"_ [^4]
 

**Why HTTPS is not enough ?**

End-to-end encryption is where the email is encrypted by the sender and decrypted by the recipient. Nobody in the middle, not the provider nor other entities have the ability to decrypt it.

With HTTPS, each email is encrypted in transit but while the intended recipient is another user, the TLS connection is initiated with a server (eg. Gmail). TLS terminates at the server, and whoever controls the server has the ability to view the messages since they are not encrypted end-to-end. Then, the email may be passed on encrypted over TLS again to the recipient.

The key difference is that the provider is able to view the email in this case. So in order to protect the content of the email from any middle person, including your email provider, we need to ensure end-to-end encryption so that only the recipient can decrypt the email.


## PGP

**History**

Pretty Good Privacy (PGP) is an encryption program that provides cryptographic privacy and authentication for data communication. PGP is used for signing, encrypting, and decrypting texts, e-mails, files, directories, and whole disk partitions and to increase the security of e-mail communications.

_"Originally designed as a human rights tool, [Phil Zimmermann](https://philzimmermann.com/EN/background/index.html) developed PGP and published it for free on the Internet in 1991. This made Zimmermann the target of a three-year criminal investigation, because the government held that US export restrictions for cryptographic software were violated when PGP spread worldwide._

_Despite the lack of funding, the lack of any paid staff, the lack of a company to stand behind it, and despite government persecution, PGP nonetheless became the most widely used email encryption software in the world."_  [^5]

After the government dropped its case in early 1996, Zimmermann founded PGP Inc, which was later acquired by Symantec Corp in 2010. Due to the patent issues mentioned earlier, PGP was not always practical for international use. That’s why the [OpenPGP Working Group](https://www.openpgp.org/about/) was formed within the Internet [Engineering Task Force (IETF)](https://www.ietf.org/). This eliminated the need to license PGP and get around some obsolete laws in the US at the time.

 [GnuPG](https://www.gnupg.org/) is a free and open-source implementation of the OpenPGP standard. It was originally developed as a replacement for Symantec's PGP proprietary software suite. Modern versions of PGP are interoperable with GnuPG and other OpenPGP-compliant systems

So to summarise, we have:

- PGP, proprietary of Symantec Corp
- OpenGPG is an open standard for email encryption and digital signature
- GnuPG is a free and open-source implementation of the OpenPGP standard 

In 2014, [the NSA was not able to break OpenGPG encrypted emails](http://www.theverge.com/2014/12/28/7458159/encryption-standards-the-nsa-cant-crack-pgp-tor-otr-snowden).

**How does OpenPGP work**

OpenPGP uses a combination of symmetric and asymmetric encryption algorithms to protect the data. It is similar to how HTTPS works, except that HTTPS encryption ends on the server of your email provider, whereas OpenPGP provides end-to-end encryption, so only the recipient can decrypt your email using his/her private key.

The basic process of encrypting and decrypting data using OpenPGP is as follows:

1.  The sender generates a symmetric key (also known as a session key) to encrypt the data.
2.  The sender encrypts the data using the symmetric key.
3.  The sender encrypts the symmetric key using the recipient's public key.
4.  The encrypted data and the encrypted symmetric key are sent to the recipient.

When the recipient receives the message, they use their private key to decrypt the symmetric key, and then use the symmetric key to decrypt the data.
![How to use PGP with Proton Mail](../assets/PGP.webp "How to use PGP with Proton Mail")
[^6]

**How to use PGP**

PGP can be used to encrypt, sign and verify signatures of texts, documents, directories, or even applications. In this section we are going to cover emails only. They are mainly two types of encryption, web encryption and local-only encryption:

- web encryption:
    
    - using a browser extension: if your webmail provider do not offer built-in encryption, you can use browser extensions like [Mailvelope](https://mailvelope.com/) or [FlowCrypt](https://flowcrypt.com/). However, while these extensions provide email encryption in transit to protect the emails while they are being sent, they do not protect them from being read by the email provider or other third parties with access to the email provider's servers.

    - using email providers that offer built-in encryption for both the email address and the webmail service like [ProtonMail](https://proton.me/), [Hushmail](https://www.hushmail.com/), [Tutanota](https://tutanota.com/) or [Posteo](https://posteo.de/en) provides a more secure solution as they encrypt the emails both in transit and at rest on their servers. [Riseup](https://riseup.net/en) at the moment of this writing does not provide end-to-end encryption but they are also committed to their user's privacy and worth mentioning.

- local-only encryption:

    - some email client applications support OpenPGP directly, which makes the process of sending encrypted emails incredibly easier. [Thunderbird](https://www.thunderbird.net/) is an open source email client developed by the Mozilla Foundation. You can follow this [online guide about how to use OpenGPG in Thunderbird](https://support.mozilla.org/en-US/kb/openpgp-thunderbird-howto-and-faq)

    - you can also use extensions for email clients that do not offer built-in PGP integration. [Enigmail](https://www.enigmail.net) was originally developed for Mozilla Thunderbird for this purpose, however Thunderbird no longer supports Enigmail since it has now built-in OpenGPG integration, but you can still use Enigmail for email clients that do not have OpenPGP by default
    
    - using a standalone application: [GnuPG](https://www.gnupg.org/index.html) is a command line tool that supports email encryption. It act as a "vault" for managing encryption keys and keeping an inventory of public keys of your contacts. Note that it is a general-purpose encryption tool, which can be used for encrypting any kind of data, not just emails.

**metadata encryption**

The current implementation of OpenPGP encrypts the body of the email but it does not hide the metadata, such as the subject, recipients, and sender fields. This is one of the limitations of OpenPGP, the metadata is not encrypted, it's sent in the clear and visible to anyone who intercepts the email message.

ProtonMail, Hushmail, Tutanota or Posteo, among some other webmail providers, offer additional layer of security like zero-access encryption which encrypts metadata, but it's not a standard feature of the OpenPGP protocol. Thunderbird however can encrypt the body of the email using OpenPGP encryption but not the metadata. 

**Encryption on the web browser vs local encryption**

Encryption on the web browser, such as the cloud services, webmail providers, or web extensions, may not be as secure as local encryption in certain scenarios.

When using cloud services, webmail providers, or browser extensions for encryption, the security of the encryption also depends on the security of the service or extension itself. They may not be as thoroughly audited or as well-vetted as traditional, locally-installed encryption software. A vulnerability in the service or extension could potentially allow an attacker to bypass the encryption. Also because the encryption keys are stored on the device or in the browser (in cookies), and can potentially be accessed by an attacker who gains access to the device or the browser.

On the other hand, local encryption is performed on the user's device and the encryption keys are stored locally. This means that the keys are not transmitted over the network, and are less likely to be intercepted by an attacker.

It is worth noting that browser-based encryption services and extensions can be useful for users who need to access their encrypted data from multiple devices, or for users who don't have the technical expertise to set up and use local encryption software. But for high-security scenarios, it is generally recommended to use local encryption software and to be aware of the limitations of browser-based encryption services and extensions.

**Forwarding emails**

Using an encrypted email client like Thunderbird in combination with a provider like Gmail to forward your emails is not a good practice, because your emails will be encrypted on your device, but they will be exposed while they are in transit and while they are stored on Gmail's servers. 

# Conclusions

Overall the security and privacy of the options discussed here can vary and depend on the specific implementation, so you should always evaluate the security of these options before adopting them.

In a nutshell we can say that:

- When using email providers that do not offer built-in encryption, your emails are encrypted in transit if your connection uses HTTPS, but they are in the clear while at rest on your provider's server, so this is not a secure option
- In opposition, email clients like Thunderbird or webmail providers like ProtonMail offer built-in encryption at rest in your mailbox
- Which means, in a scenario where you want to protect your emails but none of your contacts use encrypted emails, the emails your exchange with them would not be end-to-end encrypted but they would still be encrypted at rest in your mailbox
- Generally, it is considered more secured to use local-only encryption with email clients or standalone applications like Thunderbird and GnuPG, over using web-based encryption
- However, since Thunderbird uses OpenGPG it does not encrypt metadata, whereas ProtonMail offers additional layer of security which also encrypts the metadata
- Webmail is handy for users who need to access their encrypted data from multiple devices, especially since Thunderbird does not offer mobile apps at the moment

# Inspiration & References


[^1]: Wikipedia, _"Data at rest"_, [link](https://en.wikipedia.org/wiki/Data_at_rest)

[^2]: Wikipedia, _"Cryptography"_, [link](https://en.wikipedia.org/wiki/Cryptography)

[^3]: Internet Society, _"TLS basics"_, [link](https://www.internetsociety.org/deploy360/tls/basics/)

[^4]: Cryptoparty, _"Cryptoparty Handbook"_, 2021,  [link](https://cryptoparty.is/handbook//index.html)

[^5]: Phil Zimmermann, [link](https://philzimmermann.com/EN/background/index.html)

[^6]: ProtonMail, _"How to use PGP with Proton Mail"_, [link](https://proton.me/support/how-to-use-pgp)

This article was partly written with the help of [ChatGPT by openAI](https://chat.openai.com/chat).
