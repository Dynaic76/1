### Practical 3: Playfair Cipher****
**Overview:**  
The Playfair Cipher is a digraph substitution cipher that encrypts pairs of letters (digraphs) using a 5x5 matrix generated from a secret key. This practical demonstrates encryption and decryption using the Playfair algorithm.

**Code:**
```python
def generate_matrix(key):
    key = key.replace("J", "I").upper()
    matrix = []
    used_chars = set()
    for char in key:
        if char not in used_chars and char.isalpha():
            matrix.append(char)
            used_chars.add(char)
    for char in "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if char not in used_chars:
            matrix.append(char)
            used_chars.add(char)
    return [matrix[i:i+5] for i in range(0, 25, 5)]

def find_position(matrix, char):
    for i, row in enumerate(matrix):
        if char in row:
            return i, row.index(char)
    return None

def process_text(text):
    text = text.replace("J", "I").upper()
    text = ''.join([c for c in text if c.isalpha()])
    processed = ""
    i = 0
    while i < len(text):
        if i == len(text) - 1:
            processed += text[i] + 'X'
            i += 1
        elif text[i] == text[i+1]:
            processed += text[i] + 'X'
            i += 1
        else:
            processed += text[i] + text[i+1]
            i += 2
    return processed

def playfair_encrypt(text, key):
    matrix = generate_matrix(key)
    text = process_text(text)
    encrypted = ""
    for i in range(0, len(text), 2):
        row1, col1 = find_position(matrix, text[i])
        row2, col2 = find_position(matrix, text[i+1])
        if row1 == row2:
            encrypted += matrix[row1][(col1 + 1) % 5] + matrix[row2][(col2 + 1) % 5]
        elif col1 == col2:
            encrypted += matrix[(row1 + 1) % 5][col1] + matrix[(row2 + 1) % 5][col2]
        else:
            encrypted += matrix[row1][col2] + matrix[row2][col1]
    return encrypted

def playfair_decrypt(text, key):
    matrix = generate_matrix(key)
    decrypted = ""
    for i in range(0, len(text), 2):
        row1, col1 = find_position(matrix, text[i])
        row2, col2 = find_position(matrix, text[i+1])
        if row1 == row2:
            decrypted += matrix[row1][(col1 - 1) % 5] + matrix[row2][(col2 - 1) % 5]
        elif col1 == col2:
            decrypted += matrix[(row1 - 1) % 5][col1] + matrix[(row2 - 1) % 5][col2]
        else:
            decrypted += matrix[row1][col2] + matrix[row2][col1]
    return decrypted.replace("X", "")

text = input("Enter text: ")
key = input("Enter key: ")
encrypted = playfair_encrypt(text, key)
print("Encrypted text:", encrypted)
decrypted = playfair_decrypt(encrypted, key)
print("Decrypted text:", decrypted)
```
