# DNS Theory and Security Analysis

## 1. What is DNS?

DNS stands for Domain Name System. It translates human-readable domain names into IP addresses that computers use to communicate.

Example:
- Domain: www.google.com
- IP address: An IPv4 or IPv6 address returned by DNS.

DNS is commonly called the phonebook of the internet.

## 2. Why is DNS Important?

- Translates domain names into IP addresses.
- Helps devices locate web servers.
- Supports email delivery.
- Provides domain aliases.
- Helps applications discover network services.

## 3. Complete DNS Resolution Process

Suppose a user enters `https://www.github.com` in a browser.

**Step 1: URL Entry**

The user enters a website address into the browser.

**Step 2: Browser Cache**

The browser checks whether it already has a valid cached DNS result.

**Step 3: Operating System Cache**

If the browser does not have the result, the operating system checks its DNS cache.

**Step 4: Recursive DNS Resolver**

The device sends a query to its configured DNS resolver. The resolver checks its cache.

**Step 5: Root DNS Server**

If the answer is not cached, the resolver queries a root server, which directs it to the appropriate TLD nameserver.

**Step 6: TLD Server**

The TLD server provides information about the domain's authoritative nameservers.

Example: `.com`.

**Step 7: Authoritative DNS Server**

The authoritative server provides the requested DNS record, such as an A or AAAA record.

**Step 8: DNS Response**

The resolver returns the answer to the client and may cache it according to its TTL.

The browser can now connect to the returned IP address.

### DNS Resolution Flow

Client → Browser Cache → OS Cache → Recursive Resolver → Root Server → TLD Server → Authoritative Server → DNS Response.

Note: The recursive resolver normally performs the root, TLD and authoritative queries on behalf of the client.

## 4. Types of DNS Queries

### Recursive Query
The client asks the DNS resolver to obtain a complete answer or return an error.

### Iterative Query
A DNS server returns the best information it has, often referring the requester to another DNS server.

### Non-recursive Query
The DNS server already has the required information and can return an answer directly.

## 5. DNS Record Types

| Record | Purpose |
|---|---|
| A | Maps a domain to an IPv4 address |
| AAAA | Maps a domain to an IPv6 address |
| CNAME | Creates an alias for another domain |
| MX | Identifies mail servers |
| NS | Identifies authoritative nameservers |
| PTR | Maps an IP address to a domain name |
| TXT | Stores text information |
| SOA | Contains DNS zone administrative information |
| SRV | Identifies service hosts and ports |

### A Record
Maps a hostname to an IPv4 address.

### AAAA Record
Maps a hostname to an IPv6 address.

### CNAME Record
Creates an alias pointing to another domain name.

Example: `www.example.com` may point to `example.com`.

### MX Record
Identifies the mail servers responsible for receiving email for a domain.

### NS Record
Identifies the authoritative nameservers for a domain.

### TXT Record
Stores text information, often used for domain verification and email security.

## 6. What is TTL?

TTL stands for Time To Live.

It specifies how long a DNS record can remain in a cache before it should be refreshed.

Example:

TTL = 300 seconds = 5 minutes.

A lower TTL means the record expires sooner. A higher TTL means it can remain cached longer.

## 7. DNS Protocols and Ports

| Protocol | Port | Purpose |
|---|---|---|
| UDP | 53 | Common DNS queries and responses |
| TCP | 53 | Zone transfers and certain DNS responses |
| TCP | 853 | DNS over TLS |
| HTTPS | 443 | DNS over HTTPS |

Traditional DNS commonly uses UDP port 53. TCP is also used when required.

## 8. DNS Security Attacks

### 8.1 DNS Spoofing

DNS spoofing occurs when an attacker provides a fraudulent DNS response to redirect a user to a malicious IP address.

**Example:** A user tries to visit a banking website but is redirected to an attacker-controlled website.

**Impact:**
- Phishing
- Credential theft
- Malware infection
- Traffic redirection

**Detection:**
- Unexpected DNS responses.
- Suspicious IP addresses returned for trusted domains.
- DNS responses inconsistent with trusted resolvers.

**Mitigation:**
- Use trusted DNS resolvers.
- Implement DNSSEC validation where supported.
- Monitor DNS responses.
- Secure network infrastructure.
- Validate HTTPS certificates.

### 8.2 DNS Cache Poisoning

DNS cache poisoning occurs when an attacker inserts fraudulent DNS records into a resolver's cache.

Subsequent users may receive malicious IP addresses from the poisoned cache.

**Impact:**
- Website redirection.
- Credential theft.
- Traffic interception.

**Detection:**
- Unexpected DNS records.
- Suspicious cached responses.
- Differences between trusted DNS answers.

**Mitigation:**
- Enable DNSSEC validation.
- Update DNS software.
- Use transaction-ID and source-port randomisation.
- Restrict access to DNS infrastructure.
- Monitor DNS cache changes.

### 8.3 DNS Tunnelling

DNS tunnelling abuses DNS queries and responses to transfer hidden data.

Attackers may use it for command-and-control communication or data exfiltration.

**Impact:**
- Data theft.
- Malware communication.
- Bypassing certain network controls.

**Detection:**
- Unusually long DNS queries.
- Random-looking subdomains.
- High DNS query frequency.
- Unusual TXT record activity.
- Repeated requests to suspicious domains.

**Mitigation:**
- Monitor DNS traffic.
- Implement DNS filtering.
- Restrict unnecessary DNS traffic.
- Use DNS security monitoring.
- Investigate abnormal query patterns.

### 8.4 DNS Amplification Attack

DNS amplification is a type of Distributed Denial-of-Service (DDoS) attack.

An attacker sends DNS queries with a spoofed victim IP address, causing DNS servers to send larger responses to the victim.

**Impact:**
- Network congestion.
- Bandwidth exhaustion.
- Service disruption.

**Detection:**
- High inbound DNS response traffic.
- Unsolicited UDP responses.
- Sudden network traffic spikes.

**Mitigation:**
- Apply DNS rate limiting.
- Implement anti-spoofing controls.
- Disable open recursive DNS resolvers.
- Apply network filtering.
- Use DDoS protection services.

### 8.5 DNS Hijacking

DNS hijacking occurs when an attacker changes DNS settings or compromises DNS infrastructure to redirect users.

**Impact:**
- Website redirection.
- Phishing.
- Malware distribution.

**Detection:**
- Unexpected DNS configuration changes.
- Unknown DNS servers.
- Suspicious domain resolution.

**Mitigation:**
- Protect router and DNS administrator credentials.
- Enable MFA.
- Monitor DNS configuration changes.
- Secure domain registrar accounts.
- Review DNS records regularly.

## 9. DNS Attack Comparison

| Attack | Main Objective |
|---|---|
| DNS Spoofing | Provide fraudulent DNS responses |
| DNS Cache Poisoning | Corrupt DNS cache records |
| DNS Tunnelling | Transfer hidden data through DNS |
| DNS Amplification | Overwhelm a victim with DNS responses |
| DNS Hijacking | Modify DNS resolution or configuration |

## 10. DNSSEC – DNS Security Extensions

DNSSEC adds cryptographic authentication to DNS data.

It helps DNS resolvers verify that DNS responses are authentic and have not been modified.

Important:
- DNSSEC provides data integrity and origin authentication.
- DNSSEC does not encrypt DNS traffic.
- DNSSEC does not prevent every DNS attack.

## 11. DNS Investigation from a SOC Analyst Perspective

When investigating suspicious DNS activity, a SOC analyst should:

1. Identify the suspicious domain.
2. Identify the source device and internal IP address.
3. Review DNS query frequency and patterns.
4. Inspect DNS responses and returned IP addresses.
5. Correlate DNS activity with firewall and endpoint logs.
6. Check for malware indicators and suspicious outbound connections.
7. Block confirmed malicious domains where appropriate.
8. Isolate compromised endpoints when necessary.
9. Preserve evidence and document the investigation.

## 12. Common DNS Interview Questions

**Q1. What is DNS?**

DNS translates human-readable domain names into IP addresses.

**Q2. Difference between A and AAAA records?**

A maps a domain to IPv4, whereas AAAA maps it to IPv6.

**Q3. What is DNS cache poisoning?**

It is the insertion of fraudulent DNS records into a resolver's cache.

**Q4. What is DNS tunnelling?**

It is the abuse of DNS queries and responses to transfer hidden data.

**Q5. How can a SOC analyst detect DNS tunnelling?**

By examining long queries, random-looking subdomains, unusual query frequency and suspicious TXT record activity.

**Q6. Does DNSSEC encrypt DNS traffic?**

No. DNSSEC provides authentication and integrity, not confidentiality.

**Q7. Why does DNS use UDP?**

UDP has low overhead and is efficient for typical DNS queries. TCP is used when necessary.

**Q8. What happens if DNS resolution fails?**

The hostname cannot be resolved into an IP address, so the browser generally cannot establish a connection to that domain.

## 13. Key Takeaways

- DNS translates domain names into IP addresses.
- DNS resolution involves caching and recursive/iterative queries.
- DNS records provide different types of information.
- TTL controls DNS cache duration.
- DNS can be exploited through spoofing, cache poisoning, tunnelling, amplification and hijacking.
- SOC analysts monitor DNS activity, investigate suspicious patterns and implement security controls.

## Conclusion

Understanding DNS is essential for networking, cybersecurity and SOC operations.

This study covers DNS architecture, resolution, record types, common security attacks, detection techniques and mitigation strategies.
