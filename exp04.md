# Block Cipher Modes of Operation (AES)

**Purpose:** To observe and compare different modes of operation for a block cipher (AES). This experiment demonstrates why some modes, like ECB, are insecure, while others, like CBC, provide much stronger security for encrypting messages longer than a single block.

### Prerequisites

1.  **A Modern Web Browser:** (e.g., Chrome, Firefox, Edge)
2.  **Internet Connection:** We will be using an online tool to perform the encryption and decryption.

---

### Step 1: Open the Online Encryption Tool

This experiment can be performed without writing any code by using a web-based tool.

1.  Open your web browser.
2.  Navigate to a reliable online AES tool. A good option is **[Devglan AES Encryption Tool](https://www.devglan.com/online-tools/aes-encryption-decryption)**.

---

### Step 2: Perform Encryption using ECB Mode (The Insecure Way)

Let's see why ECB mode is weak by encrypting a message with repeating parts.

1.  **Enter the Plaintext:** In the "Enter Plain Text to Encrypt" box, type the following message. Notice that the word "akshat" is repeated.
    ```
    akshat is learning security. akshat is a student.
    ```

2.  **Configure the Settings:**
    * **Select Cipher Mode of Encryption:** Choose `ECB`.
    * **Select Padding:** Choose `PKCS5Padding`.
    * **Key Size in Bits:** Select `128`.
    * **Enter Secret Key:** Enter a 16-character key. For example: `MySecretKey12345`. **(AES-128 requires a 16-character key).**
    * **Output Text Format:** Select `Base64`.

3.  **Encrypt the Message:** Click the **Encrypt** button.

4.  **Analyze the Output:**
    * You will see the encrypted text (ciphertext) in the "AES Encrypted Output" box.
    * Even though it looks random, if you analyze this ciphertext, you would find repeating patterns because the two "akshat" blocks were encrypted into the same value. This is the weakness of ECB.

---

### Step 3: Perform Encryption using CBC Mode (The Secure Way)

Now, let's encrypt the *exact same message* with the *exact same key* but using the much more secure CBC mode.

1.  **Enter the Plaintext:** Use the same message as before:
    ```
    akshat is learning security. akshat is a student.
    ```

2.  **Configure the Settings:**
    * **Select Cipher Mode of Encryption:** This time, choose `CBC`.
    * **Enter IV (Optional):** CBC requires an "Initialization Vector" to be secure. Enter a 16-character random text here. For example: `MyRandomVector123`.
    * **Key, Key Size, Padding, Output Format:** Keep all other settings the same as in Step 2 (`PKCS5Padding`, `128` bits, `MySecretKey12345`, `Base64`).

3.  **Encrypt the Message:** Click the **Encrypt** button.

4.  **Analyze the Output:**
    * Compare the new ciphertext with the one from ECB mode.
    * You will notice that this ciphertext has no repeating patterns, even though the original message did. This is because each block was chained with the previous one, making the output secure.

---

### Step 4: How to Decrypt the Message

You can also verify that the message can be decrypted back to the original text.

1.  Copy the `Base64` ciphertext you generated (either from ECB or CBC).
2.  Paste it into the "Enter Encrypted Text" box on the right side of the tool.
3.  **Crucially, make sure all the settings (Mode, Key, Key Size, Padding, and IV) on the decryption side exactly match the ones you used for encryption.**
4.  Click the **Decrypt** button. You will see your original message, "akshat is learning security...", in the output box.

---

### Step 5: Conclusion

By performing these steps, you have demonstrated the core concept of block cipher modes of operation:
* **ECB** is simple but insecure because it encrypts identical plaintext blocks into identical ciphertext blocks, leaking patterns.
* **CBC** is secure because it uses an Initialization Vector (IV) and block chaining to ensure that even identical plaintext blocks produce completely different ciphertext blocks, thus hiding any patterns.
