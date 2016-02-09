# Protocol Definition

The programming protocol uses group telegrams with the mostly unused group address 15/7/255. Those telegrams use DPT 60000, which is ureserved for manufacturer specific extensions. The used Property Data Type is PDT_GENERIC_14, which is 14 bytes of raw data. 

## Telegram overview

<table>
    <tr>
        <th>Byte no#:</th>
        <td align="center">0</td>
        <td align="center">1</td>
        <td align="center">2</td>
        <td align="center">...</td>
        <td align="center">13</td>
    </tr>
    <tr>
        <th>Used for:</th>
        <td align="center" colspan="2">Header, 2 bytes</td>
        <td align="center" colspan="3">Body, 12 Bytes</td>
    </tr>
    <tr>
        <th>Description:</th>
        <td>Protocolversion</td>
        <td align="center" >Message-Type-ID</td>
        <td align="center" colspan="3" rowspan="2">depends on Message-Type-ID</td>
    </tr>
    <tr>
        <th>Range:</th>
        <td align="center">0x00..0xFF</td>
        <td align="center">0x00..0xFF</td>        
    </tr>
</table>

**Glossary**	

* **HI**	high byte, left most byte												
* **LO**	low byte, rigt most byte												
* **GA**	group address, 1/2/3												
* **individual address**	aka. physical address, 1.2.3												

## Protocol Versions

- [Version 0x00](protocol_0x00.md)
