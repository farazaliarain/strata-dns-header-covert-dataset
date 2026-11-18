# STRATA-DNS header covert channel dataset

Packet captures of eight DNS covert channel techniques that write into the DNS header and the question structure, not into the query name. Each capture holds one technique, so the folder name is the label.

The dataset accompanies the article:

F. Ali, M. Niazi, and M. Behzad, "STRATA-DNS: Tier-aware hybrid learning for detecting DNS tunneling and header covert channels," submitted for publication, 2026.

## Contents

| Folder | File | Fields written | Packets | Queries | Windows |
|---|---|---|---:|---:|---:|
| `opcode` | `CovertOpcode.pcapng` | OPCODE, the ten unassigned values | 100,000 | 50,000 | 782 |
| `rcode` | `CovertRCodes.pcapng` | RCODE in queries, the fifteen non-zero values | 59,993 | 30,000 | 469 |
| `qclass` | `CovertQClass.pcapng` | QCLASS, values other than IN | 76,500 | 38,250 | 598 |
| `counts` | `CovertCounts.pcapng` | QDCOUNT, ANCOUNT, NSCOUNT and ARCOUNT | 192,241 | 96,150 | 1,503 |
| `flags` | `CovertHeaderFlags.pcapng` | AA and TC set in queries | 60,000 | 24,000 | 375 |
| `zflag` | `5kZflag=1.pcapng` to `5kZflag=7.pcapng` | Z, AD and CD, the seven non-zero values | 70,000 | 35,000 | 547 |
| `txid_plain` | `CovertTXIDPlains.pcapng` | Transaction identifier, plaintext bytes | 37,224 | 18,612 | 291 |
| `txid_encrypted` | `CovertTXIDEncrypted.pcapng` | Transaction identifier, encrypted bytes | 37,224 | 18,612 | 291 |
| Total | 14 files, 81 MB | | 633,182 | 310,624 | 4,856 |

"Queries" counts UDP DNS messages with QR = 0. "Windows" is the number of 64-query windows the article builds from each folder.

The number in a `zflag` file name is the value of the three bits read as (Z, AD, CD). For example, `5kZflag=1` sets CD only, `5kZflag=4` sets Z only and `5kZflag=7` sets all three. Each file holds 5,000 queries.

## How the captures were made

- Two laboratory hosts on an isolated private network. The client is 192.168.232.133 and the server is 192.168.232.130.
- The client sends each query over UDP from port 53 to port 53. The server's replies are in the captures as well.
- The capture filter kept only traffic between these two hosts on port 53, so the files contain no other traffic.
- Every technique uses one constant query name of the form `sNNN.kfuprn.edu.sa`. It is a placeholder used only inside the laboratory. The query type is A.
- Recorded with Wireshark (dumpcap) 4.6.8 between 29 August and 10 September 2026. Format: pcapng, Ethernet link type.
- No traffic was sent outside the laboratory network.

## Labels

The folder name is the technique label for every packet in that folder. Which values a technique writes is listed in the table above and, in full, in the codebook table of the article.

Not every query of a technique carries a covert value. In the `counts` capture half of the queries do, and in the `flags` capture 62.5% do. The rest are ordinary queries that act as padding or as the cleared state of a one-bit symbol.

The `flags` capture also holds 9,000 client messages with the QR bit set and 3,000 TCP segments to port 53. The article uses the UDP messages with QR = 0.

## Building the windows used in the article

1. Read the UDP DNS messages of one folder and keep the queries (QR = 0), in capture order.
2. Cut them into consecutive windows of 64 queries.
3. Keep the last, shorter window if it has at least 16 queries.

This gives the window counts in the table.

## What is not included

- The packet generator that produced the traffic is not released.
- Benign traffic and name-tunneling traffic used in the article come from third-party datasets, which are available from their original publishers and are not redistributed here.

## Integrity

`SHA256SUMS.txt` lists the SHA-256 checksum of every capture file.

## License

Creative Commons Attribution 4.0 International (CC BY 4.0). You may use and share the data if you credit the authors by citing the article above.

## Contact

Faraz Ali, Department of Information and Computer Science, King Fahd University of Petroleum and Minerals, Dhahran, Saudi Arabia. E-mail: g202416820@kfupm.edu.sa
