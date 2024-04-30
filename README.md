# Translator-APP

import numpy as np

def levenshtein_distance(word1, word2):
    """
    Calculate the Levenshtein distance between two words.
    """
    m, n = len(word1), len(word2)
    # Create a matrix to store distances
    dp = np.zeros((m + 1, n + 1), dtype=int)
    # Initialize the matrix
    for i in range(m + 1):
        dp[i, 0] = i
    for j in range(n + 1):
        dp[0, j] = j
    # Compute distances
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            cost = 0 if word1[i - 1] == word2[j - 1] else 1
            dp[i, j] = min(dp[i - 1, j] + 1,      # deletion
                           dp[i, j - 1] + 1,      # insertion
                           dp[i - 1, j - 1] + cost)  # substitution
    return dp[m, n]

def autocorrect(word, word_list):
    """
    Find the closest match to the given word from the list of words.
    """
    min_distance = float('inf')
    closest_word = None
    for candidate in word_list:
        distance = levenshtein_distance(word.strip(), candidate)
        if distance < min_distance:
            min_distance = distance
            closest_word = candidate
    return closest_word

# Example usage
word_list = ["apple", "banana", "orange", "grape", "kiwi" ,"python", "programming", "language", "computer", "code", "algorithm", "data", "science", "machine", "learning"]
user_input = input("Enter something: ")
print("You entered:", user_input)
corrected_word = autocorrect(user_input, word_list)
print(f"Autocorrected word: {corrected_word}")
