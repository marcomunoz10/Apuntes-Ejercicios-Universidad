To properly detect when there are **missing passwords**, we need to handle cases where multiple separators exist, and make sure we correctly check if there's anything **after the first separator**.

### **Fixed `awk` Command**:
Let's adjust the logic to account for multiple separators, ensuring it checks if there's anything after the first separator:
```bash
awk -F '[:;| ]+' '{ if ($2 == "" || $2 ~ /^[[:space:]]*$/) print $0 }' * | wc -l
```

### **How It Works:**
1. **`-F '[:;| ]+'`** → This uses a **regular expression** to match **one or more** of the separators (`:` `;` `|` and spaces), ensuring that we correctly handle cases with multiple consecutive separators.
2. **`$2 == ""`** → Checks if there is **nothing** after the first separator (no password). This ensures cases like `user@example.com:` are properly detected.
3. **`$2 ~ /^[[:space:]]*$/`** → This checks if the second field is **empty or only contains spaces** (including cases where multiple separators are used).
4. **`print $0`** → Prints the whole line if the condition is true (i.e., no password found after the separator).

### **Example Scenarios**

| Input Line                       | Should Match? | Why?                                      |
|-----------------------------------|---------------|-------------------------------------------|
| `user@example.com:`               | ✅ Yes        | No password after the separator `:`.      |
| `user@example.com::`              | ✅ Yes        | No password after the `::` (double separator). |
| `user@example.com:cullen5`        | ❌ No         | Password exists (`cullen5`).              |
| `anastomosis1@yahoo.co.uk::a1234567` | ❌ No         | Password exists (`a1234567`).             |
| `anastomosis1@yahoo.co.uk::`      | ✅ Yes        | No password after the `::` (empty password). |

### **Count Users Without Passwords:**
```bash
awk -F '[:;| ]+' '{ if ($2 == "" || $2 ~ /^[[:space:]]*$/) print $0 }' * | wc -l
```

This will **correctly count the users without passwords**, even in cases where there are **multiple consecutive separators** like `::`.

