# Product Cipher Implementation

**Purpose:** To demonstrate the concept of a Product Cipher by combining a simple Substitution Cipher and a Transposition Cipher. This experiment shows how layering multiple encryption techniques can significantly increase security.

### Prerequisites

1.  **Python:** You need Python installed on your computer.
    * **How to check:** Open Command Prompt and type `python --version`. If you see a version number, you're good to go.
    * **If not installed:** Download and install it from [python.org](https://www.python.org/downloads/).

2.  **Visual Studio Code (VS Code):** A code editor for writing the code.
    * **Download it here:** [https://code.visualstudio.com/](https://code.visualstudio.com/)

---

### Step 1: Set Up the Project Folder

Let's create a dedicated folder for this experiment.

1.  **Open Command Prompt:** Click the Start Menu, type `cmd`, and open the "Command Prompt".

2.  **Navigate to your Desktop:** In the command prompt window, type:
    ```bash
    cd Desktop
    ```

3.  **Create a Project Folder:** Type the following command to create a new folder for our project. We'll call it `product-cipher-demo`.
    ```bash
    mkdir product-cipher-demo
    ```

4.  **Enter the Project Folder:** Move inside the folder you just created:
    ```bash
    cd product-cipher-demo
    ```

---

### Step 2: Open in VS Code and Create the Code File

Now we will open the project in our code editor and create the Python file.

1.  **Open the folder in VS Code:** In the same command prompt window, type:
    ```bash
    code .
    ```

2.  **Create a Python File:**
    * In VS Code, in the Explorer panel on the left, right-click and select "New File".
    * Name this new file `product_cipher.py` and press `Enter`.

---

### Step 3: Add the Python Code

Copy the complete Python code below and paste it into your `product_cipher.py` file. This code has been structured for better readability.

```python
# product_cipher.py
import math

# --- STAGE 1: SUBSTITUTION CIPHER (CAESAR CIPHER) ---
def substitution_encrypt(plain_text, key):
    """Encrypts text by shifting each letter by the key value."""
    cipher_text = ""
    for char in plain_text:
        # Shift the character by the key
        encrypted_char = chr(ord(char) + key)
        cipher_text += encrypted_char
    return cipher_text

def substitution_decrypt(cipher_text, key):
    """Decrypts text by shifting each letter back by the key value."""
    plain_text = ""
    for char in cipher_text:
        # Shift the character back
        decrypted_char = chr(ord(char) - key)
        plain_text += decrypted_char
    return plain_text

# --- STAGE 2: TRANSPOSITION CIPHER (COLUMNAR TRANSPOSITION) ---
def transposition_encrypt(text, key):
    """Encrypts text by writing it into a matrix and reading it out column by column."""
    num_columns = key
    num_rows = math.ceil(len(text) / num_columns)
    
    # Pad the text with a placeholder if its length is not a multiple of the key
    padded_text = text.ljust(num_rows * num_columns, '_')
    
    # Create the matrix (list of lists)
    matrix = [list(padded_text[i : i + num_columns]) for i in range(0, len(padded_text), num_columns)]
    
    # Read column by column to get the ciphertext
    cipher_text = ""
    for c in range(num_columns):
        for r in range(num_rows):
            cipher_text += matrix[r][c]
            
    return cipher_text

def transposition_decrypt(cipher_text, key):
    """Decrypts text by writing it into a matrix column by column and reading it out row by row."""
    num_columns = key
    num_rows = math.ceil(len(cipher_text) / num_columns)
    
    # Create the matrix to be filled column by column
    matrix = [['' for _ in range(num_columns)] for _ in range(num_rows)]
    
    # Fill the matrix
    char_index = 0
    for c in range(num_columns):
        for r in range(num_rows):
            if char_index < len(cipher_text):
                matrix[r][c] = cipher_text[char_index]
                char_index += 1
            
    # Read row by row to get the decrypted text
    plain_text = ""
    for r in range(num_rows):
        plain_text += "".join(matrix[r])
        
    return plain_text.rstrip('_') # Remove any padding

# --- Main Program ---
if __name__ == "__main__":
    # Get user input
    plain_text = input("Enter the plain text: ")

    # Define the keys for each stage
    substitution_key = 1
    transposition_key = 2

    print("\n--- ENCRYPTION PROCESS ---")
    # Stage 1: Apply Substitution Cipher
    substituted_text = substitution_encrypt(plain_text, substitution_key)
    print(f"1. After Substitution (key={substitution_key}): {substituted_text}")

    # Stage 2: Apply Transposition Cipher on the result of Stage 1
    final_ciphertext = transposition_encrypt(substituted_text, transposition_key)
    print(f"2. After Transposition (key={transposition_key}): {final_ciphertext}")

    print("\n--- DECRYPTION PROCESS ---")
    # Stage 1: Reverse Transposition Cipher
    transposition_decrypted = transposition_decrypt(final_ciphertext, transposition_key)
    print(f"1. After Reversing Transposition: {transposition_decrypted}")
    
    # Stage 2: Reverse Substitution Cipher
    original_text = substitution_decrypt(transposition_decrypted, substitution_key)
    print(f"2. After Reversing Substitution: {original_text}")

    print("\n-------------------------------")
    print(f"Original Text: {plain_text}")
    print(f"Final Encrypted Text: {final_ciphertext}")
    print(f"Final Decrypted Text: {original_text}")
    print("-------------------------------")
```
Step 4: Run the Program
Let's execute the script.

Open the Integrated Terminal in VS Code:

In the top menu, click on Terminal -> New Terminal.

Run the Python Script: In the terminal, type the following command and press Enter:
```
python product_cipher.py
```
Provide Input:

The program will ask you to Enter the plain text:. Type a message, for example: akshat.

Step 5: Understand the Output
After providing the input, you will see a step-by-step output of the encryption and decryption process:

Enter the plain text: akshat
```
--- ENCRYPTION PROCESS ---
1. After Substitution (key=1): bltibu
2. After Transposition (key=2): btbliu

--- DECRYPTION PROCESS ---
1. After Reversing Transposition: bltibu
2. After Reversing Substitution: akshat

-------------------------------
Original Text: akshat
Final Encrypted Text: btbliu
Final Decrypted Text: akshat
-------------------------------
```
What the output means:

Encryption Process: It shows how your original text (akshat) is first changed by the substitution cipher to bltibu, and then its positions are scrambled by the transposition cipher to produce the final encrypted text btbliu.

Decryption Process: It shows the reverse process, where the transposition is undone first to get bltibu, followed by the substitution, to successfully get the original text akshat back.
