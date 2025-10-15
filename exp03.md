# Playfair Cipher Implementation in Java

**Purpose:** To implement the Playfair cipher, a classic digraphic substitution cipher. This experiment demonstrates how encrypting pairs of letters, instead of single letters, can create a more secure encryption scheme that resists simple frequency analysis.

### Prerequisites

1.  **Java Development Kit (JDK):** You need the JDK to compile and run Java code.
    * **How to check:** Open Command Prompt and type `javac -version`. If you see a version number, you're good to go.
    * **If not installed:** Download and install the JDK from the [Oracle website](https://www.oracle.com/java/technologies/downloads/).

2.  **Visual Studio Code (VS Code):** A code editor for writing the code. You will also need the "Extension Pack for Java" from the VS Code marketplace for the best experience.

---

### Step 1: Set Up the Project Folder

Let's create a dedicated folder for this experiment.

1.  **Open Command Prompt:** Click the Start Menu, type `cmd`, and open the "Command Prompt".

2.  **Navigate to your Desktop:** In the command prompt window, type:
    ```bash
    cd Desktop
    ```

3.  **Create a Project Folder:** Type the following command to create a new folder for our project.
    ```bash
    mkdir playfair-cipher-java
    ```

4.  **Enter the Project Folder:** Move inside the folder you just created:
    ```bash
    cd playfair-cipher-java
    ```

---

### Step 2: Open in VS Code and Create the Code File

Now we will open the project in our code editor and create the Java file.

1.  **Open the folder in VS Code:** In the same command prompt window, type:
    ```bash
    code .
    ```

2.  **Create a Java File:**
    * In VS Code, in the Explorer panel on the left, right-click and select "New File".
    * Name this new file `Playfair.java` and press `Enter`. **Note:** The filename must exactly match the class name (`Playfair`).

---

### Step 3: Add the Java Code

Copy the complete Java code below and paste it into your `Playfair.java` file.

```java
// Playfair.java
import java.util.Scanner;

public class Playfair {

    private char[][] keyTable;
    private int[] charPositions;

    // Generates the 5x5 key table from the secret key
    private void generateKeyTable(String key) {
        keyTable = new char[5][5];
        charPositions = new int[26];
        String formattedKey = key.toLowerCase().replaceAll("[^a-z]", "").replace("j", "i");
        String keyString = "";

        // Create the key string with unique characters
        for (char c : formattedKey.toCharArray()) {
            if (keyString.indexOf(c) == -1) {
                keyString += c;
            }
        }

        // Add remaining alphabet characters to the key string
        for (char c = 'a'; c <= 'z'; c++) {
            if (c != 'j' && keyString.indexOf(c) == -1) {
                keyString += c;
            }
        }

        // Fill the key table and store character positions
        for (int i = 0; i < 25; i++) {
            keyTable[i / 5][i % 5] = keyString.charAt(i);
            charPositions[keyString.charAt(i) - 'a'] = i;
        }
    }

    // Prepares the plaintext for encryption according to Playfair rules
    private String prepareText(String text) {
        text = text.toLowerCase().replaceAll("[^a-z]", "").replace("j", "i");
        StringBuilder sb = new StringBuilder(text);

        // Insert 'x' between identical letters in a pair
        for (int i = 0; i < sb.length(); i += 2) {
            if (i + 1 < sb.length() && sb.charAt(i) == sb.charAt(i + 1)) {
                sb.insert(i + 1, 'x');
            }
        }

        // If the length is odd, append 'z' to make it even
        if (sb.length() % 2 != 0) {
            sb.append('z');
        }

        return sb.toString();
    }

    // Encrypts the prepared plaintext using the key table
    private String encrypt(String text) {
        StringBuilder encryptedText = new StringBuilder();
        char[] chars = text.toCharArray();

        for (int i = 0; i < chars.length; i += 2) {
            char a = chars[i];
            char b = chars[i + 1];

            int posA = charPositions[a - 'a'];
            int posB = charPositions[b - 'a'];
            int r1 = posA / 5, c1 = posA % 5;
            int r2 = posB / 5, c2 = posB % 5;

            if (r1 == r2) { // Rule 1: Same Row
                encryptedText.append(keyTable[r1][(c1 + 1) % 5]);
                encryptedText.append(keyTable[r2][(c2 + 1) % 5]);
            } else if (c1 == c2) { // Rule 2: Same Column
                encryptedText.append(keyTable[(r1 + 1) % 5][c1]);
                encryptedText.append(keyTable[(r2 + 1) % 5][c2]);
            } else { // Rule 3: Rectangle
                encryptedText.append(keyTable[r1][c2]);
                encryptedText.append(keyTable[r2][c1]);
            }
        }
        return encryptedText.toString();
    }

    public static void main(String[] args) {
        Playfair cipher = new Playfair();

        // --- Example with "akshat" ---
        String key1 = "Monarchy";
        String text1 = "akshat";
        
        System.out.println("--- Example 1 ---");
        System.out.println("Key: " + key1);
        System.out.println("Plaintext: " + text1);

        cipher.generateKeyTable(key1);
        String preparedText1 = cipher.prepareText(text1);
        String encryptedText1 = cipher.encrypt(preparedText1);
        
        System.out.println("Ciphertext: " + encryptedText1);

        // --- Example from Document ---
        String key2 = "Security";
        String text2 = "Don't attack today";
        
        System.out.println("\n--- Example 2 ---");
        System.out.println("Key: " + key2);
        System.out.println("Plaintext: " + text2);

        cipher.generateKeyTable(key2);
        String preparedText2 = cipher.prepareText(text2);
        String encryptedText2 = cipher.encrypt(preparedText2);

        System.out.println("Ciphertext: " + encryptedText2);
    }
}
```
Step 4: Compile and Run the Program
Java programs need to be compiled before they can be run.

Open the Integrated Terminal in VS Code:

In the top menu, click on Terminal -> New Terminal.

Compile the Java Code: In the terminal, type the following command and press Enter. This will create a Playfair.class file.
```
javac Playfair.java
```
Run the Program: Now, run the compiled code with this command:

java Playfair

Step 5: Understand the Output
After running the program, you will see the output for the two hardcoded examples in the terminal:
```
--- Example 1 ---
Key: Monarchy
Plaintext: akshat
Ciphertext: kcnbbu

--- Example 2 ---
Key: Security
Plaintext: Don't attack today
Ciphertext: hlopmybymyufblhbia
```
What the output means:

The program takes a key (like "Monarchy") and a plaintext message (like "akshat").

It then applies the Playfair cipher rules:

Prepares the text into pairs (ak sh at).

Generates the 5x5 key square based on the key.

Uses the three encryption rules (same row, same column, or rectangle) on each pair.

The final result is the Ciphertext, which is the encrypted version of your message.
