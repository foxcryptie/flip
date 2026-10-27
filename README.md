# Flip (TryHackMe)

`flip.py` is a one-off script for the Flip room. It connects to the lab service, sends the two input lines expected by that service, reads its leaked AES-CBC ciphertext, changes one byte, and sends the ciphertext back.

The change is here:

```python
ct[15] ^= ord('t') ^ ord('s')
```

CBC decryption XORs each plaintext block with the previous ciphertext block. The expression above applies the difference between `t` and `s` to one plaintext byte. Byte 15 is hardcoded because the room's message layout is hardcoded; this script will not find the right position for a different message or service.

Set `HOST` in [`flip.py`](flip.py) to your own room instance and run `python flip.py`. The parser expects the exact `Leaked ciphertext:` text and the prompts used by the room. There are no automated tests or saved run output here.

The lesson from the room is about integrity: encrypting login data with CBC does not stop someone from changing it before the server makes an authorization decision.
