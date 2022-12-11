## <font color="#F2853F" style="font-size:24pt"> os_mbuf_adj</font>

```c
void os_mbuf_adj(struct os_mbuf *mp, int req_len);
```

Trims *req_len* bytes from either the head (if positive) or tail (if negative) of an mbuf chain. Adjusts the packet length of the mbuf chain if *mp* points to a packet header mbuf. When trimming from the head, no mbufs are freed. When trimming from the tail, any mbufs of zero length left at the end of the chain are freed.

<br>

#### Arguments

| Arguments | Description |
|-----------|-------------|
| mp |  Pointer to mbuf. Can be head of a chain of mbufs, a single mbuf or a packet header mbuf  |
| req_len | Number of bytes to trim from head or tail of mbuf


<br>

#### Returned values

None

#### Notes


#### Example

```c
    uint16_t pktlen;
	struct os_mbuf *om;
	struct my_pkt_header hdr;
	
	/* Get mbuf chain length */
	pktlen = OS_MBUF_PKTLEN(om);
	
	/* Strip header from mbuf chain */
    os_mbuf_adj(om, sizeof(struct my_pkt_header));
    pktlen -= sizeof(struct my_pkt_header);

    /* New packet length should be old packet length minus stripped header */
	assert(pktlen == OS_MBUF_PKTLEN(om));
```

