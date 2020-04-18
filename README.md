package Test;
import java.util.ArrayList;
// This is Hangman Game with Java keywords
import java.util.Random;
import java.util.Scanner;

import FinalProject.HangmanGame;
public class Game {
	
	
	public static final String[] words = {"ABSTRACT", "ASSERT", "BOOLEAN", "CATCH", "CHAR", "CLASS", "CONTINUE", 
   		    "DEFAULT", "DOUBLE", "ELSE", "EXTENDS", "FINAL", "FINALLY", "FLOAT", "IMPLEMENT", 
     		"IMPORT", "INSTANCEOF", "INTERFACE", "PACKAGE", "PROTECTED", "RETURN", "STATIC", 
     		"SUPER", "TRUE"}; 
        
    public static final Random random = new Random();
    //Define max errors before player lose
    public static final int maxErrors = 5; //lives
    private String wordToFind;
    //Word found stored in a char array to show progression of user
    private char[] wordFound;
    private int Errors;
    //letters already entered by player
    private ArrayList<String> letters = new ArrayList<>();
    
    //Method to return randomly next word to find
    private String nextWordToFind() {
    	return words[random.nextInt(words.length)];
    }
    
    //Method for starting a new game
    public void newGame() {
    	Errors = 0;
    	letters.clear();
    	wordToFind = nextWordToFind();
    	
    	//word found initialization
    	wordFound = new char[wordToFind.length()];
    	
    	for (int i = 0; i <wordFound.length; i++) {
    		wordFound[i] = '*';
      }
    }
    
    //Method returning trues if word is found by player
    public boolean wordFound() {
    	return wordToFind.contentEquals(new String(wordFound));
    }
    
    //Method updating the word found after player entered a character
    private void enter(String c) {
    	//update only if c has not already been entered
      if (!letters.contains(c)) {
    	  //we check if word to find contains c
    	  if (wordToFind.contains(c)) {
    		  //is so, we replace * by the character c
    		  int index = wordToFind.indexOf(c);
    		  
    		  while (index >= 0) {
    			  wordFound[index] = c.charAt(0);
    			  index = wordToFind.indexOf(c, index +1);
    		  }
    	  }else {
    		  //if c not in the word display error
    		  Errors++;
    	  }
    	  
    	  //c is now a letter entered
    	  letters.add(c);
      }
    }
    //Method returning the state of word found by the user until by now
    private String wordFoundContent() {
    	StringBuilder builder = new StringBuilder();
    	
    	for (int i = 0; i< wordFound.length; i++) {
    		builder.append(wordFound[i]);
    		
    	if (i < wordFound.length - 1) {
    		builder.append(" ");
    	}
    	}
    	return builder.toString();
    }
    
    //Play method for Hangman Game
    public void play() {
    	try (Scanner input = new Scanner(System.in)){
    		//can play while Errors are lower then max errors or player has found the word
    		while (Errors< maxErrors) {
    			System.out.println("Enter a letter: ");
    			//get next input from player
    			String str = input.next();
    			
    			//we keep just first letter
    			if (str.length() > 1) {
    				str = str.substring(0,1);
    			}
    			
    			//update word found
    			enter(str);
    			
    			//display current state
    			System.out.println("/n" + wordFoundContent());
    			
    			//check if word is found
    			if (wordFound()) {
    				System.out.println("You Win!!!");
    				break;
    			}else {
    				//we display number of tries remaining for the user
    				System.out.println("Tries remainig: " + (maxErrors - Errors));
    			}
    		}
    		
    	if (Errors == maxErrors) {
    		//player losed
    		System.out.println("You Lose!!!");
    		System.out.println("Word to find was: " +wordToFind);
    		}
    	}	
    }
     
    public static void main(String[] args) {
        //Greeting
        System.out.println("Welcome to Hangman Game!");
        System.out.println("The Game is based on Java Keywords.");
    HangmanGame hangmanGame = new HangmanGame();
    hangmanGame.newGame();
    hangmanGame.play();
    
    }
    	 }
        
