# Tapping IEEE-802.11 Packet Sniffer on the Socks

Tepsots is a lightweight tool using standard Python libraries only to sniff packets on a wireless network device that has been placed into monitoring mode. The Radiotap and IEEE-802.11 layer are dissected partially to provide fast processing. The following values are extracted for all packets: frame length, frame type, frame subtype, source address, destination address, sequence number, fragment number, frequency, data rate (for IEEE-802.11b/g) and received signal indicator (RSSI).

## Usage

A few arguments for packet filtering are supported. Filtering can be composed of up to six features: 1) source MAC addresses, 2) destination MAC addresses, 3) frame types, 4) frame subtypes, 5) frame length and 6) RSSI. Each feature can take either one or multiple arguments defining a minimum value or a range respectively. Setting the verbose flag shows the ASCII-encoded byte representation of the whole IEEE-802.11 frame.

Some examples are provided below.

1. Show all available information with human-readable timestamps of captured packets received on interface "wlp2s0":

```bash
$ sudo tepsots.py -i wlp2s0 -r -v
```

2. Show only packets captured on the IEEE-802.11n physical layer with at least -42 dBm:

```bash
$ sudo tepsots.py -i wlp2s0 -s -42 -v | grep ' n '
```

3. Capture Beacons and CTS packets with non-verbose output:

```bash
$ sudo tepsots.py -i wlp2s0 -t 0 1 -st 8 12
```

4. Capture packets broadcasted from any of two devices:

```bash
$ sudo tepsots.py -i wlp2s0 -sa 0102030405aa 0102030405bb -da ffffffffffff
```

## Output

```
11:44:27.229418 465 g 349 0 8 444e6d9????? None None None 2412 6.0 -67 
11:44:27.260264 15 n 115 2 8 5c49793????? 18cf5e9????? 3032 0 2412 58.5 -53 
11:44:27.261267 372 g 279 0 8 5c49793????? None None None 2412 6.0 -50 
11:44:27.273519 1216 b 152 2 0 18cf5e9????? ffffffffffff 3321 0 2412 1.0 -50 
11:44:27.275266 452 g 339 0 8 ccce1e9????? None None None 2412 6.0 -68 
```

From left to right:

1. timestamp from the system clock
2. transmission time of the packet (in us)
3. IEEE-802.11 physical layer (either "b", "g", "n", or "ac")
4. size of the packet (in bytes)
5. frame type (0=management, 1=control, 2=data)
6. frame subtype
7. source address
8. destination address
9. frame sequence number
10. frame fragment number
11. channel (MHz)
12. data rate (Mbps)
13. signal strength (RSSI)

## Credits

- Scapy Project -- [A powerful interactive packet manipulation program](https://scapy.net/)
- Radiotap -- [De facto standard for 802.11 frame injection and reception.](https://www.radiotap.org/)
