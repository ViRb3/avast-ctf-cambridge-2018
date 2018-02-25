We now have the bytes:
```
50 44 55 3E 30 30 30 31 30 30 30 31 38 31 46 30 30 30 31 30 30 43 36 31 37 36 33 39 39 43 30 45 38 46 45 39 45 31 46 32 39 43 30 45
```

We convert them to ASCII using `xxd`:
```
→ echo "50 44 55 3E 30 30 30 31 30 30 30 31 38 31 46 30 30 30 31 30 30 43 36 31 37 36 33 39 39 43 30 45 38 46 45 39 45 31 46 32 39 43 30 45" \
| xxd -p -r
PDU>0001000181F000100C6176399C0E8FE9E1F29C0E
```

An interesting output. If we Google `PDU`, we learn that it stands for `protocol data unit`.
Surely there must be some form of a decoder then?

Indeed, another Google search reveals a [PDU (SMS) decoder](https://www.diafaan.com/sms-tutorials/gsm-modem-tutorial/online-sms-pdu-decoder/)!

We decode our message and receive:
```
Text message
To:	0

Message:	
aleaiactaest

...snip...
```

Finally, the password for this level is: `aleaiactaest`
