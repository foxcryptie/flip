# TryHackMe: Flip

A challenge-specific Python script for the *Flip* lab. The service returns an AES-CBC ciphertext and later accepts a ciphertext for authentication without checking its integrity. The script changes one ciphertext byte before sending it back.

## How the script works

1. Connects to the configured host on port `1337`.
2. Sends the username and password bytes expected by this challenge.
3. Reads the line labelled `Leaked ciphertext` from the service response.
4. Applies `ct[15] ^= ord('t') ^ ord('s')` and submits the modified ciphertext.

In CBC decryption, a byte changed in the previous ciphertext block changes the corresponding byte in the next plaintext block. The XOR expression above is the difference between `t` and `s`. It depends on the exact plaintext layout used by this lab; changing the username, password, or server format can make byte 15 the wrong position.

## Run in the lab

Edit `HOST` in [`flip.py`](flip.py) to the address of your own TryHackMe instance, then run:

```bash
python flip.py
```

The script expects specific prompts and the exact `Leaked ciphertext:` label. It has no socket timeout, automated tests, or captured successful run in this repository. It is a proof of concept for this exercise, not a general AES-CBC attack tool.

## Takeaway

Encryption alone does not authenticate a message. A service must reject modified ciphertext before using decrypted data for an authorization decision.
