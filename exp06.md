# RSA Digital Signature Implementation

**Purpose:** To implement the RSA digital signature scheme using Python. This experiment demonstrates how a message can be signed with a private key to ensure its authenticity and integrity, and how that signature can be verified by anyone using the corresponding public key.

### Prerequisites

1.  **Python:** You need Python installed on your computer.
    * **How to check:** Open Command Prompt and type `python --version`.

2.  **Cryptography Library:** This experiment requires a third-party library. You will need to install it.
    * We will install this in Step 4.

3.  **Visual Studio Code (VS Code):** A code editor for writing the code.

---

### Step 1: Set Up the Project Folder

Let's create a dedicated folder for this experiment.

1.  **Open Command Prompt** and navigate to your Desktop:
    ```bash
    cd Desktop
    ```

2.  **Create and Enter the Project Folder:**
    ```bash
    mkdir rsa-digital-signature-demo
    cd rsa-digital-signature-demo
    ```

---

### Step 2: Open in VS Code and Create the Code File

Now, let's open the project in our code editor.

1.  **Open the folder in VS Code:** In the command prompt, type:
    ```bash
    code .
    ```

2.  **Create a Python File:**
    * In VS Code, create a new file and name it `digital_signature.py`.

---

### Step 3: Add the Python Code

Copy the complete Python code below and paste it into your `digital_signature.py` file.

```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes
from cryptography.exceptions import InvalidSignature

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)
public_key = private_key.public_key()

message = b"This is a secure message from akshat."
tampered_message = b"This is a FAKE message from akshat."

signature = private_key.sign(
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("--- RSA Digital Signature Demonstration ---")
print(f"\nOriginal Message: {message.decode()}")
print(f"Digital Signature (first 30 bytes in hex): {signature.hex()[:60]}...")

print("\n[VERIFICATION 1: Using the authentic message]")
try:
    public_key.verify(
        signature,
        message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )
    print("SUCCESS: The signature is valid. The message is authentic and has not been tampered with.")
except InvalidSignature:
    print("FAILURE: The signature is invalid.")

print("\n[VERIFICATION 2: Using a tampered message]")
try:
    public_key.verify(
        signature,
        tampered_message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )
    print("FAILURE: The signature was incorrectly verified for a tampered message.")
except InvalidSignature:
    print("SUCCESS: The signature is invalid. The tampered message was correctly rejected.")


```
Step 4: Install Library and Run the Program
This script requires an external library, so we need to install it first.

Open the Integrated Terminal in VS Code (Terminal -> New Terminal).

Install the cryptography library: In the terminal, type the following command and press Enter.
```
pip install cryptography
```
Run the Python Script: Once the installation is complete, run the program with this command:
```
python digital_signature.py
```
Step 5: Understand the Output
You will see the following output in your terminal, demonstrating the entire process:
```
--- RSA Digital Signature Demonstration ---

Original Message: This is a secure message from akshat.
Digital Signature (first 30 bytes in hex): 1a2b3c... (this will be a long random-looking string)

[VERIFICATION 1: Using the authentic message]
SUCCESS: The signature is valid. The message is authentic and has not been tampered with.

[VERIFICATION 2: Using a tampered message]
SUCCESS: The signature is invalid. The tampered message was correctly rejected (this is the expected behavior).
```
What the output means:

Signing: The program first creates a digital signature for the original message.

Verification 1: It successfully verifies the signature against the original message, proving it's authentic.

Verification 2: It then tries to verify the same signature against a different (tampered) message. The verification fails, which is exactly what we want. This proves that if someone alters even a single character in the message, the digital signature becomes invalid.
