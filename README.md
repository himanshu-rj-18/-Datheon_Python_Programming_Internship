# -Datheon_Python_Programming_Internship
import random

# ASCII stages for the hangman gallows (0 to 6 mistakes)
HANGMAN_STAGES = [
    """
       ------
       |    |
            |
            |
            |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
            |
            |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
       |    |
            |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
      /|    |
            |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
      /|\\   |
            |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
      /|\\   |
      /     |
            |
    =========
    """,
    """
       ------
       |    |
       O    |
      /|\\   |
      / \\   |
            |
    =========
    """
]

def get_random_word():
    """Selects a secret word. Uses a default list or can be adapted for a .txt file."""
    words = ["python", "developer", "hangman", "console", "programming", "variable", "function", "computer"]
    return random.choice(words).lower()

def play_hangman():
    word = get_random_word()
    word_letters = set(word) # letters in the word
    alphabet = set('abcdefghijklmnopqrstuvwxyz')
    guessed_letters = set() # what the user has guessed
    
    incorrect_guesses = 0
    max_incorrect = 6

    print("========================================")
    print("      Welcome to Hangman Console!       ")
    print("========================================")

    while incorrect_guesses < max_incorrect and len(word_letters) > 0:
        # Display current gallows state
        print(HANGMAN_STAGES[incorrect_guesses])
        
        # Display current word status (e.g., p _ t h o n)
        word_display = [letter if letter in guessed_letters else '_' for letter in word]
        print("Current word: " + " ".join(word_display))
        
        # Display guessed letters
        print("Guessed letters: " + ", ".join(sorted(guessed_letters)) if guessed_letters else "Guessed letters: None")
        
        # Get user input
        user_guess = input("\nGuess a letter: ").lower().strip()

        # Input validation
        if len(user_guess) != 1 or user_guess not in alphabet:
            print("❌ Invalid input. Please enter a single alphabetical letter.")
            continue
            
        if user_guess in guessed_letters:
            print(f"⚠️ You already guessed the letter '{user_guess}'. Try a different one.")
            continue

        # Add to guessed set
        guessed_letters.add(user_guess)

        if user_guess in word_letters:
            word_letters.remove(user_guess)
            print(f"✅ Good job! '{user_guess}' is in the word.")
        else:
            incorrect_guesses += 1
            print(f"❌ Incorrect! '{user_guess}' is not in the word. Mistakes: {incorrect_guesses}/{max_incorrect}")

        print("-" * 40)

    # Game Over / Victory Screens
    if len(word_letters) == 0:
        print("\n🎉 CONGRATULATIONS! You successfully guessed the word:")
        print(f"👉 {word.upper()} 👈")
    else:
        print(HANGMAN_STAGES[max_incorrect])
        print("\n💀 GAME OVER! You ran out of attempts.")
        print(f"The secret word was: {word.upper()}")

if __name__ == "__main__":
    play_hangman()
