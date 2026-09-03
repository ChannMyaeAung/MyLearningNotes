# CIDR Notation: A Simple Mental Model

One simple way I understand CIDR notation is by looking at **which parts of an IP address can change**.

An IPv4 address has 4 octets:

```text
10.0.0.0
 ↑  ↑ ↑ ↑
```

Each octet contains 8 bits, making a total of 32 bits.

## `/24`

```text
10.0.0.x/24
```

`/24` means the first 24 bits (3 octets) are the network portion.

So:

```text
10.0.0.x
```

Only the **last octet** can change.

Examples:

```text
10.0.0.1
10.0.0.42
10.0.0.100
10.0.0.254
```

## `/16`

```text
10.0.x.x/16
```

`/16` means the first 16 bits (2 octets) are the network portion.

So the last **two octets** can change.

Examples:

```text
10.0.0.1
10.0.1.1
10.0.42.100
10.0.255.254
```

## `/8`

```text
10.x.x.x/8
```

Only the first octet is fixed, while the remaining three octets can change.

Examples:

```text
10.0.0.1
10.1.2.3
10.100.50.25
10.255.255.254
```

## My Mental Model

```text
/0   →  x.x.x.x          (all addresses — the entire internet)
/8   →  10.x.x.x
/16  →  10.0.x.x
/24  →  10.0.0.x
/32  →  10.0.0.1          (single host — one specific machine)
```

The number after `/` tells us how many bits are reserved for the **network portion**.

The remaining bits can be used for addresses inside that network.

> The larger the CIDR number, the smaller the network range.
> `/0` matches everything (used in security group "allow all" rules). `/32` matches exactly one IP (used to grant access to a single server).

This visualization makes CIDR notation much easier for me to understand.