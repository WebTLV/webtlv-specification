Design Objectives
=================

Compact Content
---------------

This specification defines encoding rules for web content in ASN.1 form so as to allow its encoding in TLV form.

The intention is to allow for efficient transmission and processing of web content on deeply embedded systems such as smart cards and microcontroller-based products.

While doing so the intention is to offer near-perfect compatibility with mainline text-based content formats so that existing web technologies such as browsers can be reused.

Encoding Rules
--------------

Common encoding rules are defined so as to allow reuse of specification and implementation.

The encoding rules should allow for a compact encoding while avoiding or minimizing any loss of semantic information. Where possible the encoding rules should allow for perfect reconstruction of content previously transcoded to a WebTLV format.

Reasonable compatibility with existing standards such as GlobalPlatform should be maintained with regards to encoding choices. The ASN.1 tag space should be managed so as to avoid collisions.

Tere is no need for encodings to be a strictly canonical mapping to mainline formats. Non-canonical mappings should be included where indicated by other requirements.

Content Formats
---------------

Various formats are defined, each of which equivalent to a web content type such as XML or CSS.

Formats should be defined so that they can be readily transferred and transcoded. They should also allow for efficient processing of their encoded data structures.

