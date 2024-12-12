import random
import string

def generate_password(username, email, length=12):
    """
    Generates a password following these rules:
    - Length is between 6 and 32 characters.
    - At least one uppercase letter.
    - No more than 2 identical consecutive characters.
    - Does not contain the username or email.
    """
    if length < 6 or length > 32:
        raise ValueError('Password length must be between 6 and 32 characters.')
    
    # Allowed character set
    allowed_chars = '!#$%&()*+,-./:;<=>?@[\\]_`{|}~' + string.ascii_letters + string.digits
    
    while True:
        # Randomly select characters from the allowed set
        password = ''.join(random.choices(allowed_chars, k=length))
        
        # Check for at least one uppercase letter
        if not any(char.isupper() for char in password):
            continue
        
        # Check for no more than 2 consecutive identical characters
        if any(password[i] == password[i+1] == password[i+2] for i in range(len(password) - 2)):
            continue
        
        # Ensure the password doesn't contain the username or email
        if username.lower() in password.lower() or email.lower() in password.lower():
            continue
        
        return password


def save_password_to_file(password, file_name="passwords.txt"):
    """
    Saves the password to a file.
    """
    with open(file_name, "a") as file:
        file.write(f"Generated Password: {password}\n")
    print(f"Password saved to {file_name}")


def main():
    print("Welcome to the Secure Password Generator!")
    
    # Take inputs from the user
    username = input("Enter your username: ").strip()
    email = input("Enter your email: ").strip()
    
    while True:
        try:
            length = int(input("Enter the desired password length (between 6 and 32): "))
            if 6 <= length <= 32:
                break
            else:
                print("Password length must be between 6 and 32.")
        except ValueError:
            print("Invalid input. Please enter a number between 6 and 32.")
    
    # Generate a password
    password = generate_password(username, email, length)
    print(f"Your generated password is: {password}")
    
    # Ask the user if they want to save the password to a file
    save_to_file = input("Do you want to save this password to a file? (yes/no): ").strip().lower()
    if save_to_file == 'yes':
        save_password_to_file(password)
    
    print("Thank you for using the Secure Password Generator!")


if __name__ == "__main__":
    main()
