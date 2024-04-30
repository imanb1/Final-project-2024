# Final-project-2024
Crossword puzzle gen.
https://youtu.be/f3j7qMRq5dI 

#this is our crossword puzzle game
#it contains three different difficulty levels the user can choose from
#our theme was countries

#this is our crossword puzzle game
#it contains three different difficulty levels the user can choose from
#our theme was countries

import random
import string

class CrosswordGenerator:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.grid = [[' ' for _ in range(width)] for _ in range(height)]

    def add_word(self, word, row, col, direction):
        if direction == 'across':
            for c, letter in enumerate(word):
                self.grid[row][col + c] = letter
        elif direction == 'down':
            for r, letter in enumerate(word):
                self.grid[row + r][col] = letter

    def intersect(self, word, row, col, direction):
        if direction == 'across':
            for c, letter in enumerate(word):
                if self.grid[row][col + c] != ' ' and self.grid[row][col + c] != letter:
                    return True
                elif direction == ' down':
                    for r, letter in enumerate(word):
                        if self.grid[row + r][col] != ' ' and self.grid[row + r][col] != letter:
                            return True
                return False
    
    def fill_grid(self, words):
        random.shuffle(words)
        for word in words:
            direction = random.choice(['across', 'down'])
            if direction == 'across':
                col = random.randint(0, self.width - len(word))
                row = random.randint(0, self.height - 1)
            else:
                col = random.randint(0, self.width - 1)
                row = random.randint(0, self.height - len(word))
            if not self.intersect(word, row, col, direction):
                self.add_word(word, row, col, direction)

    def add_fillers(self):
        for i in range(self.height):
            for j in range(self.width):
                if self.grid[i][j] == ' ':
                    self.grid[i][j] = random.choice(string.ascii_uppercase)
    
    def display(self):
        print('  ' + ' '.join([str(i % 10) for i in range(self.width)]))
        for i, row in enumerate(self.grid):
            print(str(i % 10)+ ' ' + ' '.join(row))

    def check_guess(self, row, col, direction, guess):
        if direction == 'across':
            for c, letter in enumerate(guess):
                if self.grid[row][col + c] != ' ' and self.grid[row][col + c]!= letter:
                    return False
                self.grid[row][col + c] = letter
        elif direction == 'down':
            for r, letter in enumerate(guess):
                if self.grid[row + r][col] != ' ' and self.grid[row + r][col] != letter:
                    return False
                self.grid[row + r][col] = letter
            return True
    
easy_words = ['paris', 'tokyo', 'rome', 'london', 'newyork']
medium_words = ['beijing', 'istanbul', 'mumbai', 'cairo', 'sydney']
hard_words = ['rio', 'moscow', 'athens', 'berlin', 'tokyo']

def play_game():
    while True:
        print('Choose difficulty level: ')
        print('1. Easy')
        print('2. Medium')
        print('3. Hard')
        print('4. Quit')
        choice = input('Enter your choice (1-4): ')

        if choice == '1':
            width = 8
            height = 8
            words = easy_words
        elif choice == '2':
            width = 10
            height = 10
            words = medium_words
        elif choice == '3':
            width = 12
            height = 12
            words = hard_words
        elif choice == '4':
            print('Thanks for playing!')
            return
        else:
            print('invalid choice. Please enter a number between 1 and 4.')
            continue

        generator = CrosswordGenerator(width, height)
        generator.fill_grid(words)
        generator.add_fillers()
        print('Welcome to the Crossword Game!')
        print('Try to fill in the missing letters.')
        generator.display()

        while True:
            row = int(input('Enter the row number (0 - {}): '.format(height - 1)))
            col = int(input('Enter the column number (0 - {}): '.format(width - 1)))
            direction = input("Enter 'across' or 'down' for direction: ")
            guess = input('Enter your guess: ')

            if generator.check_guess(row, col, direction, guess):
                print('Correct!')
                generator.display()
            else:
                print('Sorry, that\'s incorrect. Try again. ')
            

if __name__ == '__main__':
    while True:
        play_game()
        play_again = input('Do you want to play again? (yes/no): ')
        if play_again.lower() != 'yes':
            print


        
 
