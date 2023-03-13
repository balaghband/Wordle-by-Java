# Wordle-by-Java
This the wordle game that I made by Java language as an assignment of CS152

/**
 * CS 152 Lab 5 - Wordle
 * <p>
 * Implement the methods needed to play a Wordle-like game.
 * <p>
 * Student name: BEHNOUD ALAGHBAND
 */

import java.util.Scanner;
import java.util.Random;

/**
 * Contains methods for a wordle clone
 */
public class Wordle {

    /**
     * Represents a letter in the guess in the correct position.
     */
    public static final char CORRECT = 'X';

    /**
     * Represents a letter in the guess that is present, but in wrong place.
     */
    public static final char PRESENT = 'o';

    /**
     * Represents a letter in the guess does not occur in the word at all.
     */
    public static final char MISSING = '.';

    /**
     * How many guesses do we get to solve the puzzle?
     */
    public static final int NUMBER_OF_GUESSES = 6;

    /**
     * Picks a random word from the dictionary.
     *
     * @param dictionary An array of words.
     * @return Randomly chosen word from dictionary.
     */
    public static String pickRandomWord(String[] dictionary) {
        // TODO: Implement this method
        //implementing random function
        Random r = new Random();
        // i is for every word of dictionary
        int i = r.nextInt(dictionary.length);
        //return a random word from WordleDictionary
        return dictionary[i];
    }

    /**
     * Is the guess a recognized word?
     *
     * @param guess      The guess word.
     * @param dictionary Array of known words.
     * @return True if guess is in dictionary, false if not.
     */
    public static boolean isValidWord(String guess, String[] dictionary) {
        // TODO: Implement this method
        //String guesss = guess [(int)(Math.random()*guess.length())];
        //for every i in length of dictionary array, there should be a word
        //in the dictionary.
        for (int i = 0; i < dictionary.length; i++) {
            //all guess words must be inside the dictionary to be valid.
            if (dictionary[i].equals(guess)) {
                return true;
            }
        }

        return false;
    }


    /**
     * How close is the guess to the secret word?
     *
     * @param guess Guessed word
     * @param word  The hidden word
     * @return Array of characters corresponding to the letters of guess,
     * where the char at a given index is:
     * CORRECT if the guess letter appears at that position in word
     * PRESENT if the guess letter is present in word elsewhere
     * MISSING if the guess letter does not occur in word
     */
    public static char[] getResultForGuess(String guess, String word) {
        //declaring an empty array to input correct, present and missing icons
        char[] myguess = new char[guess.length()];
        boolean[] used = new boolean[guess.length()];
        //i means every character of user input(guess)
        for (int i = 0; i < guess.length(); i++) {
            //if all characters in guess are equal to word with the same
            // positions
            if (guess.charAt(i) == word.charAt(i)) {
                myguess[i] = CORRECT;
                used[i] = true;
            //if there is no common character between guess and word
            } else {
                myguess[i] = MISSING;
            }
        }
        //if there are common character(s) between guess and word but not at
        //the same position(s), PRESENT should be printed on incorrect position
        for (int i = 0; i < guess.length(); i++) {
            for (int j = 0; j < guess.length(); j++) {
                if (!used[j]) {
                    if (guess.charAt(i) == word.charAt(j)) {
                        myguess[i] = PRESENT;
                        used[j] = true;
                    }
                }
            }
        }
        return myguess;
    }


    /**
     * Is this a winning result?
     *
     * @param guessResult Array as returned by getResultForGuess
     * @return True if all places are CORRECT, false if not
     */
    public static boolean isWinningResult(char[] guessResult) {
        // TODO: Implement this method
        for (int i = 0; i < guessResult.length; i++) {
            if (guessResult[i] != CORRECT) {
                return false;
            }
        }
        return true;

    }


    /**
     * Plays a console based Wordle game
     *
     * @param args Ignored
     */
    public static void main(String[] args) {

        System.out.println("Let's play Wordle!");
        System.out.println();
        // The big array of words is in a separate file
        String[] words = WordleDictionary.FIVE_LETTER_WORDS;

        Scanner in = new Scanner(System.in);

        String secret = pickRandomWord(words);
        int guesses = NUMBER_OF_GUESSES;

        boolean winning;

        do {
            System.out.println("What is your guess?");

            String guess = in.nextLine().trim().toLowerCase();

            while (!isValidWord(guess, words)) {
                System.out.println("Not a recognized word! Try again");
                guess = in.nextLine().trim().toLowerCase();
            }

            char[] guessResult = getResultForGuess(guess, secret);
            System.out.println(new String(guessResult));

            winning = isWinningResult(guessResult);
            guesses--;

        } while (guesses > 0 && !winning);

        System.out.println("The word was " + secret);
    }
}

