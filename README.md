# Tugas_Kriptografi_Playfair_Cipher
Nama     : Nicky Pascal Tambunan
kelas    : TI.22.A.5
NIM      : 312210474


def create_playfair_table(key):
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    key = key.upper().replace("J", "I")  # Menggabungkan huruf 'I' dan 'J'
    table = []
    used_letters = set()

    # Membuat tabel kunci
    for char in key:
        if char not in used_letters and char in alphabet:
            table.append(char)
            used_letters.add(char)

    # Lengkapi tabel dengan sisa alfabet
    for char in alphabet:
        if char not in used_letters:
            table.append(char)
            used_letters.add(char)

    # Ubah ke bentuk matriks 5x5
    return [table[i:i + 5] for i in range(0, 25, 5)]


def prepare_text(text):
    text = text.upper().replace("J", "I").replace(" ", "")
    digraphs = []
    i = 0
    while i < len(text):
        a = text[i]
        if i + 1 < len(text):
            b = text[i + 1]
            if a == b:
                digraphs.append(a + "X")
                i += 1
            else:
                digraphs.append(a + b)
                i += 2
        else:
            digraphs.append(a + "X")
            i += 1
    return digraphs


def find_position(table, char):
    for row in range(5):
        for col in range(5):
            if table[row][col] == char:
                return row, col
    return None


def encrypt_playfair(plaintext, key):
    table = create_playfair_table(key)
    digraphs = prepare_text(plaintext)
    ciphertext = []

    for digraph in digraphs:
        a_row, a_col = find_position(table, digraph[0])
        b_row, b_col = find_position(table, digraph[1])

        if a_row == b_row:
            ciphertext.append(table[a_row][(a_col + 1) % 5] + table[b_row][(b_col + 1) % 5])
        elif a_col == b_col:
            ciphertext.append(table[(a_row + 1) % 5][a_col] + table[(b_row + 1) % 5][b_col])
        else:
            ciphertext.append(table[a_row][b_col] + table[b_row][a_col])

    return ''.join(ciphertext)


def decrypt_playfair(ciphertext, key):
    table = create_playfair_table(key)
    digraphs = prepare_text(ciphertext)
    plaintext = []

    for digraph in digraphs:
        a_row, a_col = find_position(table, digraph[0])
        b_row, b_col = find_position(table, digraph[1])

        if a_row == b_row:
            plaintext.append(table[a_row][(a_col - 1) % 5] + table[b_row][(b_col - 1) % 5])
        elif a_col == b_col:
            plaintext.append(table[(a_row - 1) % 5][a_col] + table[(b_row - 1) % 5][b_col])
        else:
            plaintext.append(table[a_row][b_col] + table[b_row][a_col])

    return ''.join(plaintext)


# Main program
if __name__ == "__main__":
    key = "TEKNIK INFORMATIKA"
    
    # Plaintext yang akan dienkripsi
    plaintext_1 = "GOOD BROOM SWEEP CLEAN"
    plaintext_2 = "REDWOOD NATIONAL STATE PARK"
    plaintext_3 = "JUNK FOOD AND HEALTH PROBLEMS"

    # Enkripsi
    encrypted_text_1 = encrypt_playfair(plaintext_1, key)
    encrypted_text_2 = encrypt_playfair(plaintext_2, key)
    encrypted_text_3 = encrypt_playfair(plaintext_3, key)

    print(f"Plaintext 1: {plaintext_1}")
    print(f"Ciphertext 1: {encrypted_text_1}")
    
    print(f"Plaintext 2: {plaintext_2}")
    print(f"Ciphertext 2: {encrypted_text_2}")
    
    print(f"Plaintext 3: {plaintext_3}")
    print(f"Ciphertext 3: {encrypted_text_3}")

    # Dekripsi
    decrypted_text_1 = decrypt_playfair(encrypted_text_1, key)
    decrypted_text_2 = decrypt_playfair(encrypted_text_2, key)
    decrypted_text_3 = decrypt_playfair(encrypted_text_3, key)

    print(f"Decrypted text 1: {decrypted_text_1}")
    print(f"Decrypted text 2: {decrypted_text_2}")
    print(f"Decrypted text 3: {decrypted_text_3}")
