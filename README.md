import hashlib
from Crypto.Cipher import AES
from Crypto import Random

def pad(s):
    return s + b"\0" * (AES.block_size - len(s) % AES.block_size)

def encrypt(message, key):
    message = pad(message)
    iv = Random.new().read(AES.block_size)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    return iv + cipher.encrypt(message)

def decrypt(ciphertext, key):
    iv = ciphertext[:AES.block_size]
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = cipher.decrypt(ciphertext[AES.block_size:])
    return plaintext.rstrip(b"\0")

Пример за използване:
key = hashlib.sha256(b"my_secret_key").digest()
message = b"Hello, world! This is a secret message."
ciphertext = encrypt(message, key)
print(ciphertext)
plaintext = decrypt(ciphertext, key)
print(plaintext)
