# py
import os
import re

# Constants
USERS_FILE = 'users.txt'
DELIMITER = ','

# Regex patterns for validating input
EMAIL_PATTERN = r'^[^@]+@[^@]+\.[^@]+$'
PASSWORD_PATTERN = r'^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[!@#$%^&*()_+-=]).{5,16}$'

def login(username, password):
    """
    Attempts to log in a user with the given username and password.
    Returns True if the login is successful, False otherwise.
    """
    with open(USERS_FILE, 'r') as f:
        for line in f:
            parts = line.strip().split(DELIMITER)
            if parts[0] == username and parts[1] == password:
                return True
    return False

def register(username, password):
    """
    Registers a new user with the given username and password.
    Returns True if the registration is successful, False if the username is already taken.
    """
    if not username_exists(username):
        with open(USERS_FILE, 'a') as f:
            f.write(f'{username}{DELIMITER}{password}\n')
        return True
    return False

def username_exists(username):
    """
    Returns True if the given username is already in use, False otherwise.
    """
    with open(USERS_FILE, 'r') as f:
        for line in f:
            if line.startswith(username + DELIMITER):
                return True
    return False

def get_password(username):
    """
    Returns the password for the given username, or None if the username does not exist.
    """
    with open(USERS_FILE, 'r') as f:
        for line in f:
            parts = line.strip().split(DELIMITER)
            if parts[0] == username:
                return parts[1]
    return None

# Main program
while True:
    print('1. Login')
    print('2. Register')
    print('3. Forgot password')
    print('4. Quit')
    choice = input('Enter your choice: ')

    if choice == '1':
        # Prompt the user for a username and password
        username = input('Enter your username: ')
        password = input('Enter your password: ')

        # Attempt to log the user in
        if login(username, password):
            print('Welcome back,', username)
        else:
            print('Invalid username or password')

    elif choice == '2':
        # Prompt the user for a username and password
        while True:
            username = input('Enter your desired username (email): ')
            if not re.match(EMAIL_PATTERN, username):
                print('Invalid email format')
            elif username_exists(username):
                print('That email is already
