# DNS Packet Analysis Using Wireshark

## 1. Project Overview

**Project Name:** DNS Resolution and Packet Analysis Using Wireshark

**Tool:** Wireshark

**Operating System:** Windows

**Protocol:** DNS

**Default Port:** UDP 53

### Objective

To capture and analyse DNS traffic using Wireshark, understand DNS queries and responses, identify DNS record types, examine TTL values and understand how packet analysis helps in identifying suspicious network activity.

## 2. Lab Requirements

- Windows operating system.
- Wireshark installed.
- Active internet connection.
- Command Prompt.
- A browser.

## 3. Practical 1 – Capturing DNS Traffic

### Objective

Capture DNS queries and responses generated when resolving a domain name.

### Procedure

1. Open Wireshark.
2. Select the active Wi-Fi or Ethernet interface.
3. Start packet capture.
4. Open Windows Command Prompt.
5. Execute:

```cmd
nslookup www.github.com
```

### Command Explanation

- `nslookup` is a command-line utility used to query DNS servers.
- `www.github.com` is the hostname being resolved.

### Wireshark Display Filter

```text
dns
```

### Expected Observation

Wireshark displays DNS packets generated during name resolution.

Inspect:
- Source IP address.
- Destination IP address.
- DNS transaction ID.
- Query name.
- Query type.
- Response code.
- Answer section.

## 4. Practical 2 – Analysing DNS Queries

### Objective

Identify outgoing DNS queries.

### Display Filter

```text
dns.flags.response == 0
```

### Procedure

1. Capture DNS traffic.
2. Generate a DNS query using nslookup.
3. Apply the display filter.
4. Select a DNS query packet.
5. Expand the Domain Name System section.

### Expected Observation

A DNS query contains information about the domain name being requested and the record type being queried.

## 5. Practical 3 – Analysing DNS Responses

### Objective

Identify DNS responses returned by the resolver.

### Display Filter

```text
dns.flags.response == 1
```

### Procedure

1. Generate a DNS query.
2. Apply the response filter.
3. Select the corresponding DNS response.
4. Expand the Answers section.

### Expected Observation

The response may contain:
- A record.
- AAAA record.
- CNAME record.
- Other requested DNS information.

A response may also indicate an error or that the requested record does not exist.

## 6. Practical 4 – Analysing DNS Record Types

### A Record – IPv4

Display filter:

```text
dns.qry.type == 1
```

Purpose: Identifies DNS queries requesting IPv4 addresses.

### AAAA Record – IPv6

Display filter:

```text
dns.qry.type == 28
```

Purpose: Identifies DNS queries requesting IPv6 addresses.

### CNAME Record

Display filter:

```text
dns.qry.type == 5
```

Purpose: Identifies DNS queries requesting canonical names or aliases.

### MX Record

Display filter:

```text
dns.qry.type == 15
```

Purpose: Identifies DNS mail-server queries.

### TXT Record

Display filter:

```text
dns.qry.type == 16
```

Purpose: Identifies DNS text-record queries.

**Important:** These filters identify queries by requested record type. To inspect the actual returned record type, expand the DNS Answer section in the packet details.

## 7. Practical 5 – Understanding TTL

### Objective

Inspect the TTL value in DNS responses.

### Procedure

1. Capture DNS traffic.
2. Generate a DNS query.
3. Select the DNS response.
4. Expand Domain Name System.
5. Expand Answers.
6. Locate the Time to live field.

### Example

TTL = 300 seconds.

This means the DNS record can remain cached for five minutes.

### Expected Observation

TTL determines how long a DNS record can be cached before it should be refreshed.

## 8. Practical 6 – Analysing DNS Error Responses

### Objective

Identify unsuccessful DNS responses.

### Display Filter

```text
dns.flags.rcode == 3
```

This identifies NXDOMAIN responses.

NXDOMAIN means the queried domain name does not exist according to the DNS response.

### Expected Observation

Repeated NXDOMAIN responses can sometimes indicate:
- Typing mistakes.
- Misconfigured applications.
- Domain discovery activity.
- Suspicious malware behaviour.

NXDOMAIN activity alone does not prove malicious activity.

## 9. Practical 7 – Identifying Suspicious DNS Traffic

### Objective

Understand how DNS packet analysis can help identify suspicious network activity.

### Useful Display Filters

All DNS traffic:

```text
dns
```

DNS TXT queries:

```text
dns.qry.type == 16
```

DNS queries with long names:

```text
dns.qry.name.len > 50
```

DNS responses:

```text
dns.flags.response == 1
```

DNS traffic over UDP port 53:

```text
udp.port == 53
```

### What to Investigate

- Unusually long domain names.
- Repeated DNS queries.
- Random-looking subdomains.
- Unusual TXT record activity.
- High DNS query frequency.
- Frequent NXDOMAIN responses.
- Unexpected external DNS destinations.

These patterns may indicate DNS tunnelling, malware communication or suspicious domain discovery. Additional evidence is required before confirming an attack.

## 10. Practical 8 – DNS Cache Investigation

### Objective

Understand DNS caching in Windows.

### Command 1: Display DNS Cache

```cmd
ipconfig /displaydns
```

Purpose: Displays DNS resolver cache entries stored on the Windows device.

### Command 2: Clear DNS Cache

```cmd
ipconfig /flushdns
```

Purpose: Clears the Windows DNS resolver cache.

Note: Clearing the cache is a troubleshooting operation. It is not a substitute for investigating or remediating DNS compromise.

## 11. DNS Investigation Scenario – SOC Perspective

### Scenario

A workstation generates unusually frequent DNS queries to a suspicious domain.

### Investigation Steps

1. Identify the source IP address.
2. Identify the queried domain.
3. Check DNS query frequency.
4. Inspect the DNS record types.
5. Examine the returned IP address.
6. Correlate DNS activity with endpoint and firewall logs.
7. Check for suspicious processes and outbound connections.
8. Determine whether the activity is legitimate or malicious.

### Possible Security Concerns

- DNS tunnelling.
- Malware command-and-control communication.
- DNS reconnaissance.
- Data exfiltration.
- Suspicious domain resolution.

### Mitigation

- Block confirmed malicious domains.
- Apply DNS filtering.
- Investigate compromised endpoints.
- Monitor unusual DNS query patterns.
- Use trusted DNS resolvers.
- Preserve investigation evidence.

## 12. Common Wireshark DNS Filters – Quick Reference

| Purpose | Display Filter |
|---|---|
| All DNS packets | `dns` |
| DNS queries | `dns.flags.response == 0` |
| DNS responses | `dns.flags.response == 1` |
| A record queries | `dns.qry.type == 1` |
| AAAA record queries | `dns.qry.type == 28` |
| CNAME queries | `dns.qry.type == 5` |
| MX queries | `dns.qry.type == 15` |
| TXT queries | `dns.qry.type == 16` |
| NXDOMAIN responses | `dns.flags.rcode == 3` |
| UDP port 53 | `udp.port == 53` |
| TCP port 53 | `tcp.port == 53` |
| Long DNS query names | `dns.qry.name.len > 50` |

## 13. Key Learnings

Through this practical exercise, I learned:

1. How to capture DNS traffic using Wireshark.
2. How to distinguish DNS queries and responses.
3. How to inspect DNS record types.
4. How to identify TTL values.
5. How to inspect DNS error responses.
6. How to examine DNS traffic for suspicious patterns.
7. How to use Windows commands for DNS troubleshooting.
8. How packet analysis supports SOC investigations.

## 14. Conclusion

This practical project provided hands-on experience in capturing and analysing DNS traffic using Wireshark.

It strengthened my understanding of DNS resolution, packet structure, record types, caching and basic DNS security investigation techniques.

The skills gained are relevant to network troubleshooting, security monitoring and SOC Analyst operations.
