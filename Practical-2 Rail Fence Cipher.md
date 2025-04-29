### **Practical 2: Rail Fence Cipher**
**Overview:**  
The Rail Fence Cipher is a transposition cipher that arranges the plaintext in a zigzag pattern and reads it row-wise to produce the ciphertext. This practical demonstrates encryption and decryption using a specified depth.

**Code:**
```python
def rail_fence_enc(txt, depth):
    rail = [['\n' for _ in range(len(txt))] for _ in range(depth)]
    row, step = 0, 1
    for i in range(len(txt)):
        rail[row][i] = txt[i]
        if row == 0:
            step = 1
        elif row == depth - 1:
            step = -1
        row += step
    encrypted_text = ''.join(char for row in rail for char in row if char != '\n')
    return encrypted_text

def rail_fence_dec(enc_txt, depth):
    rail = [['\n' for _ in range(len(enc_txt))] for _ in range(depth)]
    row, step = 0, 1
    for i in range(len(enc_txt)):
        rail[row][i] = '*'
        if row == 0:
            step = 1
        elif row == depth - 1:
            step = -1
        row += step
    index = 0
    for r in range(depth):
        for c in range(len(enc_txt)):
            if rail[r][c] == '*' and index < len(enc_txt):
                rail[r][c] = enc_txt[index]
                index += 1
    result = []
    row, step = 0, 1
    for i in range(len(enc_txt)):
        result.append(rail[row][i])
        if row == 0:
            step = 1
        elif row == depth - 1:
            step = -1
        row += step
    return ''.join(result)

txt = input("Enter text to encrypt: ")
depth = int(input("Enter depth: "))
encrypted = rail_fence_enc(txt, depth)
print("Encrypted text:", encrypted)
decrypted = rail_fence_dec(encrypted, depth)
print("Decrypted text:", decrypted)
```
