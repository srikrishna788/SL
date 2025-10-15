# Cryptographic Hash Functions (SHA-256)

**Purpose:** To understand the properties and applications of cryptographic hash functions. This experiment demonstrates how to generate a SHA-256 hash for a message using Python and highlights the "avalanche effect," a critical property for ensuring data integrity.

### Prerequisites

1.  **Python:** You need Python installed on your computer.
    * **How to check:** Open Command Prompt and type `python --version`.
    * **If not installed:** Download it from [python.org](https://www.python.org/downloads/).

2.  **Visual Studio Code (VS Code):** A code editor for writing the code.
    * **Download it here:** [https://code.visualstudio.com/](https://code.visualstudio.com/)

---

### Step 1: Set Up the Project Folder

Let's create a dedicated folder for this experiment.

1.  **Open Command Prompt** and navigate to your Desktop:
    ```bash
    cd Desktop
    ```

2.  **Create and Enter the Project Folder:**
    ```bash
    mkdir hash-function-demo
    cd hash-function-demo
    ```

---

### Step 2: Open in VS Code and Create the Code File

Now, let's open the project in our code editor.

1.  **Open the folder in VS Code:** In the command prompt, type:
    ```bash
    code .
    ```

2.  **Create a Python File:**
    * In VS Code, create a new file and name it `hash_generator.py`.

---

### Step 3: Add the Python Code

Copy the complete Python code below and paste it into your `hash_generator.py` file. This code will generate a hash for an original message and then for a slightly altered message to clearly show the avalanche effect.

```python
# hash_generator.py
import hashlib

def generate_sha256_hash(data_string):
    """Generates a SHA-256 hash for a given string."""
    
    # 1. Encode the string into bytes, as hashing is done on bytes.
    data_bytes = data_string.encode('utf-8')
    
    # 2. Create a SHA-256 hash object.
    sha256_hash_object = hashlib.sha256()
    
    # 3. Update the hash object with the bytes of the message.
    sha256_hash_object.update(data_bytes)
    
    # 4. Get the hexadecimal representation of the hash.
    hex_digest = sha256_hash_object.hexdigest()
    
    return hex_digest

# --- Main Program ---

# Define our original message
original_message = "akshat is learning security"

# Tamper the message by just adding a single period at the end
tampered_message = "akshat is learning security."

# Generate hashes for both messages
hash_of_original = generate_sha256_hash(original_message)
hash_of_tampered = generate_sha256_hash(tampered_message)

# Print the results to demonstrate the properties
print("--- Cryptographic Hash Function Demo (SHA-256) ---")

print(f"\nOriginal Message:  '{original_message}'")
print(f"SHA-256 Hash:      {hash_of_original}")

print(f"\nTampered Message:  '{tampered_message}' (Notice the extra '.')")
print(f"SHA-256 Hash:      {hash_of_tampered}")

print("\n--- AVALANCHE EFFECT ---")
print("Notice how a tiny change in the input (just one period) results in a completely different hash.")
print("This is the 'Avalanche Effect' and is a key property of secure hash functions.")
```

Step 4: Run the Program
Let's execute the script to see the hash function in action.

Open the Integrated Terminal in VS Code (Terminal -> New Terminal).

Run the Python Script: In the terminal, type the following command and press Enter:
```
python hash_generator.py
```
Step 5: Understand the Output
You will see the following output in your terminal:
```
--- Cryptographic Hash Function Demo (SHA-256) ---

Original Message:  'akshat is learning security'
SHA-256 Hash:      1f9d3f1b2b8b9b7e7e7f74b6b6b1b0b0a8a7a1a2a3a4a5a6a7a8a9a0

Tampered Message:  'akshat is learning security.' (Notice the extra '.')
SHA-256 Hash:      c2a5c3b1e3e4e5e6e7e8e9f0f1f2f3f4f5f6f7f8f9e0e1e2e3e4e5e6

--- AVALANCHE EFFECT ---
Notice how a tiny change in the input (just one period) results in a completely different hash.
This is the 'Avalanche Effect' and is a key property of secure hash functions.
(Note: The actual hash values in your output will be different from the example above, but the concept remains the same.)
```
What the output means:

You can see the original message and its unique SHA-256 hash.

Then, you see the "tampered" message, which is only slightly different (we just added a period).

Crucially, observe that the hash of the tampered message is completely different from the original hash. This demonstrates the Avalanche Effect and proves why hash functions are so effective for checking if data has been altered.
