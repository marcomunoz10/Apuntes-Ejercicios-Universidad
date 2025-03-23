

```python
#!/usr/bin/env python3
import os
import re
from tqdm import tqdm
import matplotlib.pyplot as plt
from collections import Counter
import numpy as np

directory_path = "/home/kali/Desktop/GIS/PRAC1/n"

separators = r"[:;| ]"

mails = []
passwords = []
longest_password = ""
longest_password_length = 0
file_with_longest_password = ""


shortest_password = ""
shortest_password_length = 0
file_with_shortest_password = ""

all_entries = os.listdir(directory_path)

all_files = [elemento for elemento in all_entries if os.path.isfile(os.path.join(directory_path, entry))]

sum = 0


for filename in all_files:
        file_path = os.path.join(directory_path, filename)
        with open(file_path, "r", encoding="utf-8", errors="ignore") as file:
                for line in file:
                        if not line:
                                continue
                        parts = re.split(separators, line, maxsplit=1)
                        if len(parts) == 2:
                                email, password = parts

                                password = password.strip()
                                if ".txt" in password or not password.strip():
                                                                            
                                    #print(f"[-] Detectado en el fichero {filename} la contraseña {password}.")
                                    continue

                                if not password.isprintable():
                                        #print(f"[+] Detected in {filename}, the non printeable char number {sum}")
                                        #sum = sum + 1
                                        continue  # No lo añadimos

                                passwords.append(password)


total_passwords = len(passwords)
print(f"[+] Hay un total de {total_passwords} contraseñas. ")
print(f"[!] No se han podido printear {sum} contraseñas. ")



numeric_passwords = [password for password in passwords if password.isdigit()]
alphabetic_passwords = [password for password in passwords if password.isalpha()]
alphanumeric_passwords = [password for password in passwords if password.isalnum()]

segle = re.compile(r"\b(19[0-9]{2} | 20[0-9]{2})\b")
segle_passwords = [password for password in passwords if segle.findall(password)]

lowercase_passwords = [password for password in alphabetic_passwords if password.islower()]

uppercase_passwords = [password for password in alphabetic_passwords if password.isupper()]

firstupper_passwords = [pw for pw in alphabetic_passwords if pw[0].isupper() and pw[1:].islower()]


def percentage(part, whole):
    return f"{(part/whole*100):.2f}" if whole > 0 else "0"

print(f"Nº del apartado: ")

print(f"                [1] Hay {len(numeric_passwords)} contraseñas numéricas.")
print(f"                        [+] Equivalen al {percentage(len(numeric_passwords), total_passwords)} %.")

print(f"                [2] Hay {len(alphabetic_passwords)} contraseñas alfabéticas.")
print(f"                        [+] Equivalen al {percentage(len(alphabetic_passwords), total_passwords)} %.")

print(f"                [3] Hay {len(alphanumeric_passwords)} contraseñas alfanuméricas.")
print(f"                        [+] Equivalen al {percentage(len(alphanumeric_passwords), total_passwords)} %.")

print(f"                [4] Hay {len(segle_passwords)} contraseñas entre el siglo XX y XXI.")
print(f"                        [+] Equivalen al {percentage(len(segle_passwords), total_passwords)} %.")

print(f"                [5] Hay {len(lowercase_passwords)} contraseñas afabéticas con todos los carácteres en minúsculas.")
print(f"                        [+] Equivalen al {percentage(len(lowercase_passwords), total_passwords)} %.")

print(f"                [6] Hay {len(uppercase_passwords)} contraseñas alfabéticas con todos los carácteres en mayúsculas.")
print(f"                        [+] Equivalen al {percentage(len(uppercase_passwords), total_passwords)} %.")

print(f"                [7] Hay {len(firstupper_passwords)} contraseñas que empiezan en mayúsculas.")
print(f"                        [+] Equivalen al {percentage(len(firstupper_passwords), total_passwords)} %.")




```