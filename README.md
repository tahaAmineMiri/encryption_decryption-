# Transposition Cipher Encryption and Decryption

This project demonstrates the implementation of a Transposition Cipher for encrypting and decrypting messages using Python. The project includes solutions for different parts of the implementation, including an optional part that attempts to hack the cipher using brute force.

## Project Structure

## Transposition Cipher Class

The `TranspositionCipher` class is defined in multiple parts of the project. Below is a summary of its methods:

### Initialization

```python
class TranspositionCipher(object): 
        
    def __init__(self, key):
        # Set the key for the cipher
        self.key = key
```

### Encryption

```python 
def encrypt_message(self, message):
    # Convert the message to lowercase and split it into a list of characters
    message_split = list(message.lower())
    
    # Determine the length of the message
    message_length = len(message_split)

    # Initialize the encrypted message as an empty string
    message_encrypted = ''

    # Determine the number of rows in the encryption grid by calculating the  
    # ceiling value after dividing the message length by the key
    message_ceil = ceil(message_length/self.key)

    # Iterate over each cell and calculate the corresponding index in the message_split variable
    for j in range(self.key):
        for i in range(message_ceil):
            index = j + i * self.key
            # Check if the index is within the bounds of the message length
            if index < message_length:
                # Add the character at the index position to the encrypted message
                message_encrypted += message_split[index]

    # Return the encrypted message
    return message_encrypted
```

### Decryption
```python
def decrypt_message(self, message):
    # Convert the message to lowercase and split it into a list of characters
    message_split = list(message.lower())
    
    # Determine the length of the message
    message_length = len(message_split)
    
    # Determine the number of columns in the decryption grid by calculating the 
    # ceiling value after dividing the message length by the key
    message_ceil = ceil(message_length/self.key)
    
    # Calculate the number of empty (unused) cells in the grid
    num_empty_cells = self.key*message_ceil - message_length
    
    # Initialize a grid for the decrypted message having as many rows as the
    # key and as many columns as the calculated ceiling value
    message_grid = [['' for _ in range(message_ceil)] for _ in range(self.key)]
    
    # Initialize the decrypted message as an empty string
    message_decrypted = ''
    
    # Create an iterator for the split message
    iterator = iter(message_split)

    # Populate the decryption grid
    # Iterate over the rows (from 0 to key-1)
    for i in range(self.key):

        # Determine the number of columns to populate
        # If the row doesn't contain an empty cell, populate all cells from column = 0 to column = message_ceil
        if i < self.key - num_empty_cells:
            columns = message_ceil
        # If the row contains an empty cell, populate all cells from column = 0 to column = message_ceil-1
        else:
            columns = message_ceil - 1

        # Populate the row based on the number of columns calculated above
        # The iterator keeps track of the current character
        for j in range(columns):
            message_grid[i][j] = next(iterator, None)

    # Build the decrypted message from the grid
    # Read the message column by column
    for j in range(message_ceil):
        for i in range(self.key):
            message_decrypted += message_grid[i][j]

    # Return the decrypted message
    return message_decrypted
```

### Hacking the Cipher
This is just an optional part
```python
def hack_cipher(message_enc):
    '''
    This function attempts to decrypt the given encrypted message by brute force.
    Assumptions:
    - The message contains no punctuation.
    - Individual words in the message are separated by a space.
    - All words in the message are part of the English dictionary.
    '''
    
    # Iterate through each potential key from 1 to the length of the message
    for key in range(1, len(list(message_enc))+1):
        
        # Instantiate a TranspositionCipher object with the current key
        cipher = TranspositionCipher(key)
        
        # Attempt to decrypt the encrypted message using the current cipher
        message_dec = cipher.decrypt_message(message_enc)
        
        # Split the decrypted message into individual words
        message_dec_split = message_dec.split()
        
        # Initialize a list to store whether each word is in the English dictionary
        english_words = []
        
        # Iterate over each word in the decrypted message
        for i in message_dec_split:
            
            # Check if the current word exists in the English dictionary
            # If it does, append "True" to english_words; otherwise, append "False"
            english_words.append(PyDictionary.meaning(i) is not None)
        
        # Output the current key and its corresponding results for monitoring
        print(key, english_words)
        
        # If all words in the decrypted message are found in the dictionary,
        # we assume that the correct key has been found, and break the loop
        if(sum(english_words) == len(list(message_dec_split))):
            break
        
        # Print a blank line for readability
        print()
    
    # Return the decrypted message and the key that successfully decrypted it
    return message_dec, key
```
### Usage 
To use the ```TranspositionCipher``` class, you can create an instance of the class with a specific key and call the ```encrypt_message``` and ```decrypt_message methods```.

```python
cipher = TranspositionCipher(6)
encrypted_message = cipher.encrypt_message('Learning Python is fun')
print(f'Encrypted Message: {encrypted_message}')

decrypted_message = cipher.decrypt_message(encrypted_message)
print(f'Decrypted Message: {decrypted_message}')
```

To hack the cipher, you can call the ```hack_cipher``` function with the encrypted message.

```python
decrypted_message, key = hack_cipher('lnh egofa nurp nnyiits')
print(f'Decrypted Message: {decrypted_message} with key: {key}')
```

### Requirements
- Python 3.8.8
- PyDictionary

### Installation
```
pip install PyDictionary
```