
>>> from collections import Counter
>>> data = [
...     "Anaigflaux@yandex.ru:indonesia",
...     "Anaigin@mail.ru:s222ru",
...     "Anaiginger109@yahoo.com:",
...     "Anaigois@hotmail.com:anusca",
...     "Anaigonzalez15@yahoo.com:love1515",
...     "Anaigraves@yahoo.com:Teresa23",
...     "Anaigribeiro@hotmail.com:filipa",
...     "Anaiguimaraes@colinhillier.wanadoo.co.uk:joaomeireles",
...     "Anaiguimaraes@colinhillrier.wanadoo.co.uk:joaomeireles"
... ]
>>> separators = r"[;:| ]"
... 
... # Create an empty list to store extracted passwords
... passwords = []
... 
... # Process each line in the data to extract the password
... for entry in data:
...     parts = re.split(separators, entry.strip(), maxsplit=1)
...     if len(parts) == 2:  # Check if the line contains an email-password pair
...         password = parts[1]  # Extract the password (second part)
...         if password:  # Ensure the password is not empty
...             passwords.append(password)
...             
>>> password_counts = Counter(passwords)
... 
... # Get the most common passwords
... common_passwords = password_counts.most_common()
... 
... # Display the most common passwords and their counts
... print("\nMost Common Passwords:\n")
... for password, count in common_passwords:
...     print(f"{password}: {count}")
...     

Most Common Passwords:

joaomeireles: 2
indonesia: 1
s222ru: 1
anusca: 1
love1515: 1
Teresa23: 1
filipa: 1



```python
#!/usr/bin/env python3

import os
import re
from collections import Counter
from tqdm import tqdm  # Import tqdm for the progress bar

# Define the directory containing your files
directory_path = "/home/kali/Desktop/GIS/PRAC1/n"

# Define the separators for email-password pairs (you specified these)
separators = r"[;:| ]"

# Create an empty list to store extracted passwords
passwords = []

# Get a list of all files and directories inside the dataset directory
all_entries = os.listdir(directory_path)  # Lists all files and folders

# Filter to only include files (ignore directories)
all_files = [entry for entry in all_entries if os.path.isfile(os.path.join(directory_path, entry))]

# Loop through each file and extract passwords
for filename in tqdm(all_files, desc="Processing Files", unit="file"):
    file_path = os.path.join(directory_path, filename)
    with open(file_path, "r", encoding="utf-8", errors="ignore") as file:
        for line in file:
            parts = re.split(separators, line.strip(), maxsplit=1)
            if len(parts) == 2:  # Check if the line contains an email-password pair
                password = parts[1]  # Extract the password (second part)

                if password:  # Ensure the password is not empty
                    passwords.append(password)

# Count the occurrences of each password
password_counts = Counter(passwords)

# Get the 30 most common passwords
common_passwords = password_counts.most_common(30)

password_list, count_list = zip(*common_passwords)  # This unpacks the tuples into two lists

# Display the top 30 most common passwords and their counts
print("\nTop 30 Most Common Passwords:\n")
for password, count in zip(password_list, count_list):
    print(f"{password}: {count}")

```


Salida:
![[Pasted image 20250305165323.png]]

---


Con gráfico:
```python
#!/usr/bin/env python3

import os
import re
from collections import Counter
from tqdm import tqdm  # Import tqdm for the progress bar
import matplotlib.pyplot as plt  # Import matplotlib for graph plotting

# Define the directory containing your files
directory_path = "/home/kali/Desktop/GIS/PRAC1/n"

# Define the separators for email-password pairs (you specified these)
separators = r"[;:| ]"

# Create an empty list to store extracted passwords
passwords = []

# Get a list of all files and directories inside the dataset directory
all_entries = os.listdir(directory_path)  # Lists all files and folders

# Filter to only include files (ignore directories)
all_files = [entry for entry in all_entries if os.path.isfile(os.path.join(directory_path, entry))]

# Loop through each file and extract passwords
for filename in tqdm(all_files, desc="Processing Files", unit="file"):
    file_path = os.path.join(directory_path, filename)
    with open(file_path, "r", encoding="utf-8", errors="ignore") as file:
        for line in file:
            parts = re.split(separators, line.strip(), maxsplit=1)
            if len(parts) == 2:  # Check if the line contains an email-password pair
                password = parts[1]  # Extract the password (second part)
                if password:  # Ensure the password is not empty
                    passwords.append(password)

# Count the occurrences of each password
password_counts = Counter(passwords)

# Get the 30 most common passwords
common_passwords = password_counts.most_common(30)

# Unpack the result to get passwords and their count
password_list, count_list = zip(*common_passwords)  # This unpacks the tuples into two lists

# Display the top 30 most common passwords and their counts
print("\nTop 30 Most Common Passwords:\n")
for password, count in zip(password_list, count_list):
    print(f"{password}: {count}")

# Plotting the bar chart for the top 30 most common passwords
plt.figure(figsize=(10, 6))  # Set the size of the plot
plt.barh(password_list, count_list, color='skyblue')  # Horizontal bar chart

# Adding labels and title
plt.xlabel('Count')
plt.ylabel('Password')
plt.title('Top 30 Most Common Passwords')

# Show the plot
plt.tight_layout()  # Adjust layout to fit labels
plt.show()

```

![[Pasted image 20250305172311.png]]