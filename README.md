# File Encryptor/Decryptor

## Overview

The **File Encryptor/Decryptor** is a C++ application designed to securely encrypt and decrypt files using a Caesar cipher algorithm. It supports batch processing of files in a directory or single-file operations via command-line arguments. The project demonstrates proficiency in object-oriented programming, file handling, and task management in C++17, with a modular design that separates concerns for maintainability and scalability.

The application reads an encryption key from a `.env` file and applies it to encrypt or decrypt files. It includes two executables:
- `encrypt_decrypt`: Processes all files in a specified directory.
- `cryption`: Handles a single file via a command-line task string.


## Features

- **Batch Processing**: Encrypts or decrypts all files in a user-specified directory recursively.
- **Single-File Processing**: Supports encryption/decryption of a single file via command-line input.
- **Caesar Cipher Algorithm**: Uses a key from `.env` to shift file bytes for encryption (`byte + key`) or decryption (`byte - key`).
- **Task Management**: Queues tasks using a `std::queue` of `std::unique_ptr<Task>`, ensuring efficient and safe memory management.
- **Error Handling**: Robust checks for file access, invalid inputs, and filesystem errors using exceptions.
- **Modular Design**: Organized into modules for file handling (`fileHandling`), task processing (`processes`), and encryption/decryption (`encryptDecrypt`).

## Project Structure

```
src/
├── app/
│   ├── encryptDecrypt/
│   │   ├── Cryption.hpp        # Declares encryption/decryption function
│   │   ├── Cryption.cpp        # Implements encryption/decryption logic
│   │   ├── CryptionMain.cpp    # Standalone program for single-file processing
│   ├── fileHandling/
│   │   ├── IO.hpp             # Declares IO class for file operations
│   │   ├── IO.cpp             # Implements file handling
│   │   ├── ReadEnv.cpp        # Reads encryption key from .env
│   ├── processes/
│   │   ├── ProcessManagement.hpp  # Declares task queue management
│   │   ├── ProcessManagement.cpp  # Implements task queuing and execution
│   │   ├── Task.hpp           # Defines Task struct and Action enum
main.cpp                        # Main program for batch processing
.env                            # Stores encryption key (e.g., 12345)
Makefile                        # Builds executables: encrypt_decrypt, cryption
```


## Installation

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Create `.env` File**:
   - Create a `.env` file in the project root with an integer key, e.g.:
     ```
     12345
     ```

3. **Build the Project**:
   ```bash
   make
   ```
   - This generates two executables: `encrypt_decrypt` and `cryption`.

## Usage

### Batch Processing (`encrypt_decrypt`)
Encrypt or decrypt all files in a directory:
```bash
./encrypt_decrypt
```
- Prompts for:
  - Directory path (e.g., `./data`).
  - Action (`encrypt` or `decrypt`).
- Example:
  ```
  Enter the directory path: ./data
  Enter the action (encrypt/decrypt): encrypt
  ```
- Processes all files in `./data` recursively, applying the Caesar cipher with the key from `.env`.

### Single-File Processing (`cryption`)
Encrypt or decrypt a single file:
```bash
./cryption "filePath,action"
```
- Example:
  ```bash
  ./cryption "input.txt,ENCRYPT"
  ```
- Encrypts `input.txt` using the key from `.env`.

## How It Works

1. **Key Reading**:
   - `ReadEnv` reads the integer key from `.env` (e.g., `12345`).
2. **Task Creation**:
   - `main.cpp` creates `Task` objects for each file, specifying the file path and action (`ENCRYPT` or `DECRYPT`).
   - `Task` encapsulates a file stream (`std::fstream`), action (`Action` enum), and path.
3. **Task Management**:
   - `ProcessManagement` queues tasks using `std::unique_ptr<Task>` and executes them by calling `executeCryption`.
4. **Encryption/Decryption**:
   - `executeCryption` parses tasks using `Task::fromString`, applies the Caesar cipher, and modifies files in-place.
5. **File Handling**:
   - `IO` class manages file opening/closing, providing streams for processing.

-