# Poker-Game

//simulation of a poker game

public class CardGame {
	
  public static void createDeck(String[] cards, String[] faces, String[] suits) {
		int n = 0;
		
	for (int a = 0; a < 4; a++) {
			for (int i = 0; i < faces.length; i++) {
				cards[n] = faces[i] + " of " + suits[a];
				n = n + 1;				
			}	
		}
	}
	
	public static void swap(String[] list, int i, int j) {
		String temp = list[i];
		list[i] = list[j];
		list[j] = temp;
	}
	
  public static int randomNumber() {
		int a = (int)(Math.random() * (52));
		return a;
	}
	
	public static void shuffle(String[] cards) {
		for (int n = 0; n < 100; n++) {
			swap(cards, randomNumber(), randomNumber());
		}
	}
	
	public static void printDeck(String[] cards) {
		System.out.println("Deck: ");
		
		for (int n = 0; n < 52; n++) {
			System.out.println(cards[n]);
		}
	}
	
	public static int getHand(String[] hand, String[] cards, int[] gone, int x) {
		for (int n = 0; n < 5; n++) {
			int a = randomNumber();
			boolean alreadyDealed = false;
			
			for (int c = 0; c < 52; c++) {
				if (alreadyDealed == true) {
					c = 0;
					//restarts the loop
				}
			
				alreadyDealed = ((a == gone[c]) && (a != 0));

				if (alreadyDealed == true) {
					a = randomNumber();
					c = 0;
					//restarts loop checking each value
				}
			}
			
			gone[x] = a;
			//a has been dealed, it is now part of the cards that are gone (are not available to be dealt anymore)
			x = x + 1;
			
			hand[n] = cards[a];
		}
		return x;
		//the index of the Cards Gone array is returned and stored
	}
	
	public static void printHand(String[] hand, int x) {
		System.out.println("\nHand " + x + ": ");
		
		for (int n = 0; n < 5; n++) {
			System.out.println(hand[n]);
		}
		
		System.out.println();
	}
	
	public static void getSuit(String[] hand, String[] cardSuit) {
		for (int n = 0; n < 5; n++) {
			String[] splitHand = hand[n].split("\\s+");
			//the card is split by spaces into its rank, 'of', and its suit
			cardSuit[n] = splitHand[2];
		}
	}
	
	public static void getRank(String[] hand, String[] cardRank, int[] numRank) {
		for (int n = 0; n < 5; n++) {
			String[] splitHand = hand[n].split("\\s+");
			cardRank[n] = splitHand[0];
			if (cardRank[n].equals("Ace")) {
				numRank[n] = 1;
			} else if (cardRank[n].equals("Deuce")) {
				numRank[n] = 2;
			} else if (cardRank[n].equals("Three")) {
				numRank[n] = 3;
			} else if (cardRank[n].equals("Four")) {
				numRank[n] = 4;
			} else if (cardRank[n].equals("Five")) {
				numRank[n] = 5;
			} else if (cardRank[n].equals("Six")) {
				numRank[n] = 6;
			} else if (cardRank[n].equals("Seven")) {
				numRank[n] = 7;
			} else if (cardRank[n].equals("Eight")) {
				numRank[n] = 8;
			} else if (cardRank[n].equals("Nine")) {
				numRank[n] = 9;
			} else if (cardRank[n].equals("Ten")) {
				numRank[n] = 10;
			} else if (cardRank[n].equals("Jack")) {
				numRank[n] = 11;
			} else if (cardRank[n].equals("Queen")) {
				numRank[n] = 12;
			} else {
				numRank[n] = 13;
			}
		}
	}
	
	public static void printSuit(String[] cardSuit) {
		for (int n = 0; n < 5; n++) {
			System.out.println(cardSuit[n]);
		}
		
		System.out.println();
	}
	
	public static void printRank(String[] cardRank, int[] numRank) {
		for (int n = 0; n < 5; n++) {
			System.out.println(cardRank[n] + " " + numRank[n]);
		}
		
		System.out.println();
	}
	
	public static int hasPairs(String[] cardRank) {
		boolean hasPair = false;
		int numPairs = 0;

		for (int x = 1; x < 5; x++) {
			hasPair = cardRank[0].equals(cardRank[x]);
			if (hasPair == true) {
				numPairs = numPairs + 1;
			}
		}

		for (int x = 2; x < 5; x++) {
			hasPair = cardRank[1].equals(cardRank[x]);
			if (hasPair == true) {
				numPairs = numPairs + 1;
			}
		}

		for (int x = 3; x < 5; x++) {
			hasPair = cardRank[2].equals(cardRank[x]);
			if (hasPair == true) {
				numPairs = numPairs + 1;
			}
		}
		
		hasPair = cardRank[3].equals(cardRank[4]);
		if (hasPair == true) {
			numPairs = numPairs + 1;
		}
		
		return numPairs;
	}

	public static boolean royalPotential(int[] numRank) {
		int n = 0;
		boolean royal = false;
		
		for (int i = 0; i < 5; i++) {
			if ((numRank[i] == 1) || (numRank[i] == 10) || (numRank[i] == 11) || (numRank[i] == 12) || (numRank[i] == 13)) {
				n = n + 1;
			}
		}
		//if a hand was a Royal Flush, it would need to have an ace, a king, a queen, an jack and a ten
		
		if (n == 5) {
			royal = true;
		}
		
		return royal;
	}
	
	public static boolean isStraight(int numPairs, int[] numRank) {
		int highest = numRank[0];
		int high = 0;
		int middle = 0;
		int low = 0;
		int lowest = 0;
		boolean isStraight = false;
		
		if (numPairs == 0) {
			for (int a = 0; a < 5; a++) {
				if (numRank[a] > highest) {
					highest = numRank[a];
					for (int b = 0; b < 5; b++) {
						if (highest - numRank[b] == 1) {
							high = numRank[b];
							for (int c = 0; c < 5; c++) {
								if (high - numRank[c] == 1) {
									middle = numRank[c];
									for (int d = 0; d < 5; d++) {
										if (middle - numRank[d] == 1) {
											low = numRank[d];
											for (int e = 0; e < 5; e++) {
												if (low - numRank[e] == 1) {
													//low and numRank[e] must be consecutive
													//numRank[e] must be one smaller than low
													lowest = numRank[e];
												}
											}
										}
									}
								}
							}
						}
					}
				}
			}
		}
		
		if ((highest != 0) && (high != 0) && (middle != 0) && (low != 0) && (lowest != 0)) {
			isStraight = true;
		}
		/*these variables are all initialized to zero, but if they are all consecutive (forming a straight)
		*they would all be given a non-zero value*/

		return isStraight;
	}
	
	public static boolean isFlush(String[] cardSuit) {
		boolean flush = false;
		String suit = cardSuit[0];
		int n = 0;
		
		for (int i = 0; i < 5; i++) {
			if (cardSuit[i].equals(suit)) {
				n = n + 1;
			}
		}
		
		if (n == 5) {
			flush = true;
		}
		
		return flush;
	}
	
	public static String getCombo(int numPairs, boolean straight, boolean flush, boolean royal) {
		String combo = " ";
		
		if ((royal == true) && (flush == true)) {
			combo = "Royal Flush";
		} else if ((straight == true) && (flush == true)) {
			combo = "Straight Flush";
		} else if (numPairs == 6) {
			combo = "Four of a Kind";
		} else if (numPairs == 4) {
			combo = "Full House";
		} else if (flush == true) {
			combo = "Flush";
		} else if (straight == true) {
			combo = "Straight";
		} else if (numPairs == 3) {
			combo = "Three of a Kind";
		} else if (numPairs == 2) {
			combo = "Two Pair";
		} else if (numPairs == 1) {
			combo = "Pair";
		} else {
			combo = "High Card";
		}
		
		return combo;
	}
	
	public static int comboPoints(String combo) {
		int p = 0;

		if (combo.equals("Royal Flush")) {
			p = 10;
		} else if (combo.equals("Straight Flush")) {
			p = 9;
		} else if (combo.equals("Four of a Kind")) {
			p = 8;
		} else if (combo.equals("Full House")) {
			p = 7;
		} else if (combo.equals("Flush")) {
			p = 6;
		} else if (combo.equals("Straight")) {
			p = 5;
		} else if (combo.equals("Three of a Kind")) {
			p = 4;
		} else if (combo.equals("Two Pair")) {
			p = 3;
		} else if (combo.equals("Pair")) {
			p = 2;
		} else {
			p = 1;
		}

		return p;
	}
	
	public static void whoWins(String[] combo) {
		int maxHandIndex = 0;
		
		for (int i = 0; i < 10; i++) {
			if (comboPoints(combo[i]) > comboPoints(combo[0])) {
				maxHandIndex = i;
			}
		}
	
		System.out.println("\nWinner: Hand " + (maxHandIndex + 1));
	}
	
	public static void getHands(String[] hand1, String[] hand2, String[] hand3, String[] hand4, String[] hand5, String[] hand6, String[] hand7, String[] hand8, String[] hand9, String[] hand10, String[] cards, int[] gone, int goneIndex, String[] combo, String[] cardRank, String[] cardSuit, int[] numRank, int numPairs) {
		String[] hand = new String[5];
		
		for (int i = 0; i < 10; i++) {
			if (i == 0) {
				hand = hand1;
			} else if (i == 1) {
				hand = hand2;
			} else if (i == 2) {
				hand = hand3;
			} else if (i == 3) {
				hand = hand4;
			} else if (i == 4) {
				hand = hand5;
			} else if (i == 5) {
				hand = hand6;
			} else if (i == 6) {
				hand = hand7;
			} else if (i == 7) {
				hand = hand8;
			} else if (i == 8) {
				hand = hand9;
			} else {
				hand = hand10;
			}
			
			goneIndex = getHand(hand, cards, gone, goneIndex);
			printHand(hand, (i + 1));
			
			getSuit(hand, cardSuit);
			getRank(hand, cardRank, numRank);
			combo[i] = getCombo(hasPairs(cardRank), isStraight(numPairs, numRank), isFlush(cardSuit), royalPotential(numRank));
			System.out.println(combo[i]);
		}
	}
	
	public static void main(String args[]) {
		String[] faces = {"Ace", "Deuce", "Three", "Four", "Five", "Six", 
				"Seven", "Eight", "Nine", "Ten", "Jack", "Queen", "King"};
		String[] suits = {"Hearts", "Diamonds", "Clubs", "Spades"};
		String[] cards = new String[52];
		String[] hand1 = new String[5];
		String[] hand2 = new String[5];
		String[] hand3 = new String[5];
		String[] hand4 = new String[5];
		String[] hand5 = new String[5];
		String[] hand6 = new String[5];
		String[] hand7 = new String[5];
		String[] hand8 = new String[5];
		String[] hand9 = new String[5];
		String[] hand10 = new String[5];
		int[] gone = new int[52];
		int goneIndex = 0;
		String[] cardRank = new String[5];
		String[] cardSuit = new String[5];
		int numPairs = 0;
		int[] numRank = new int[5];
		String combo1 = " ";
		String combo2 = " ";
		String[] combo = new String[10];
		
		createDeck(cards, faces, suits);
		
		shuffle(cards);
		
		printDeck(cards);
		
		getHands(hand1, hand2, hand3, hand4, hand5, hand6, hand7, hand8, hand9, hand10, cards, gone, goneIndex, combo, cardRank, cardSuit, numRank, numPairs);
	
		whoWins(combo);
	}
}
