# Welcome to Zach Muse's CS-499 ePortfolio!

### The Journey
 
  Even though I have been out of school for about 10 years and in a career not related to Computer Science, I decided I wanted to go back and improve myself and provide a better life for my family. I originally went to school for Computer Aided Design/Drafting and was in that career field for 13 years before deciding I need to move on. Though, this year and summer have been an extreme struggle for me, as I went through a house sale, a house rehabilitation, a new baby born, and a starting a new career within all this timeframe of finishing schooling.  The journey has so far taken close to 3 years now but managing school and having three children to provide for has taken most of my sanity. I have enjoyed the process and the classes I have taken and will be pursuing a career in Software Development.
	
   The capstone course is a good introduction on time management and how to get everything completed with a set of guidelines and a timeframe. Having free reign on this type of project is a good example to employers of the future that you can relate to illustrate your project management and goal completion. The github experience has proven beneficial as a lot of prospective careers are looking for some sort of source tree/github/bitbucket-type experience.I have been successful during the period of this class of securing a Sofware Developer position and just started this passed week. Already, I have learned so much in the little timeframe that I have been there. I am thankful for taking these steps in my life and glad I submitted the application to schooling even though it flipped my world upside down.
   
   Thank you for taking the time to look through a sample of my work.
   ### -Zach Muse
  
# Algorithms

### Code Review:

[![Alt text](https://img.youtube.com/vi/Ky7Wm0DV6wI/0.jpg)](https://www.youtube.com/watch?v=Ky7Wm0DV6wI)

   For the algorithms portion of the project, I chose to modify the game that I made for IT-312, the Advanced Programming class. I took the class in the spring of this year and it went well for me. The project was to choose from a list of predefined games and work out how to code them. The game is one of rolling multiple dice with a point system and is called Farkle. I was able complete the original project as intended and it worked just fine.
   
   I have chosen this coding project because I am very proud of it. It is the most complex project that I have written front to back without a starter piece of code that was given to add-on to. It is also my first real C++ project on my own as well. So, the project is special in my heart as it works as intended and shows my real hard work. The enhancement that I have included was a way to keep track of the highest scores that won the game. It loads the highest scores from an external file and checks whether the current high score is greater than the saved highest score and if it is then it will write the new highest score to the file.
   
   The planned enhancements that I laid out in the beginning of the class have been meet as it saves the high scores to an external file and is able to retrieve it. I made further work on also getting the program to save the same of the player along with the score so it has a more arcade high score feel to it. I have it saving the name and score within one line of the text file, when it retreives the score, the algorithm drops the first 5 characters which would hold the intials of the player. It then saves the score as an INT to be able to compare to the scores later. The largest struggle for this was figuring out the best way to save the name and the score in the same file and be able to retreive it for comparison. Meaning, I wasn't sure how I should save both pieces of data, but a chose a simple approach. Saving the initials forces three characters as I did not want to run into errors of retreiving the wrong scores. The rest of the project was just refreshing myself on how to load and save to external files with C++ but I was able to research that quickly.


### Code:
```c++
// farkle.cpp : This file contains the 'main' function. Program execution begins and ends there.
//
/*
Zach Muse
IT-312
SNHU
Original 6/22/2019 


Version 2.0 Release

rules.txt to be stored in .exe file path.

*/


#include "pch.h"
#include <iostream>
#include <ctime>
#include <cstdlib>
#include <string>
#include <array>
#include <vector>
#include <fstream>

using namespace std;

//Roll Function
int roll(int diceNum)
{		
	int side = (rand() % 6 + 1);
	cout << "Die #" << diceNum+1 << " rolls a " << side << endl;
	return(side);
}
//Score Function
int score(int sideVal[], int& scoredDie, int rolls) {
	
	vector<int> sd;

	//Each side's individual score
	int score1 = 0, score2 = 0, score3 = 0, score4 = 0, score5 = 0, score6 = 0;
	
	int turnScore = 0;
	//Used to keep track of # of times a side appears in a roll
	int num1 = 0,
		num2 = 0,
		num3 = 0,
		num4 = 0,
		num5 = 0,
		num6 = 0;
	
	for  (int i = 0; i < rolls; ++i) {
		//Side 1
		if (sideVal[i] == 1) {
			score1 = score1 + 100;
			++num1;
			++scoredDie;
			sd.push_back(sideVal[i]);
			cout << "Score 100!\n";
			if (num1 == 3) {
				score1 = 1000;
				cout << "Score 1000!!!\n";				
			}
		}
		//Side 2
		else if (sideVal[i] == 2) {
			++num2;
			
			if (num2 == 3) {
				score2 = 200;
				scoredDie = scoredDie + 3;
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);				
				cout << "Score 200!\n";				
			}
		}
		//Side 3
		else if (sideVal[i] == 3) {
			++num3;
			if (num3 == 3) {
				score3 = 300;
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				scoredDie = scoredDie + 3;
				cout << "Score 300!\n";				
			}
		}
		//Side 4
		else if (sideVal[i] == 4) {
			++num4;
			if (num4 == 3) {
				score4 = 400;
				scoredDie = scoredDie + 3;
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				cout << "Score 400!\n";				
			}
		}
		//Side 5
		else if (sideVal[i] == 5) {
			score5 = score5 + 50;
			cout << "Score 50!\n";
			++scoredDie;			
			sd.push_back(sideVal[i]);
			++num5;
			if (num5 == 3) {
				score5 = 500;				
				cout << "Score 500!\n";				
			}
		}
		//Side 6
		else if (sideVal[i] == 6) {
			++num6;
			if (num6 == 3) {
				score6 = 600;
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				sd.push_back(sideVal[i]);
				scoredDie = scoredDie + 3;
				cout << "Score 600!\n";				
			}
		}
	}		
		
	cout << "Number of Scored dice: " << scoredDie << endl;
	
	turnScore = score1 + score2 + score3 + score4 + score5 + score6;
	

	cout << "Total for this roll: " << turnScore << endl;
	vector<int>::iterator it;
	for (it = sd.begin(); it != sd.end(); it++) {
		cout << "Scoring numbers: "<< *it <<endl;
	}
	
	return(turnScore);
}
//Load Rules
void ruleText() {
	string line;
	ifstream rules("rules.txt"); //Text file in program location
	if (rules.is_open()) {

		//Read Text file, line by line
		while (getline(rules, line)) {
			cout << line << '\n';
		}
		rules.close();
		//return 0;

	}
	//Display failure message if cannot find file
	else cout << "Error loading file!";
}

//Load High Scores
void highScores(int curScore) {
	int highScore = 0;
	string initials;
	string line;
	ifstream scores("scores.txt"); //Text file in program location
	if (scores.is_open()) {
		
		//Read Text file, line by line
		while (getline(scores, line)) {
			
			string highscore_str = line.substr(6);
			highScore = stoi(highscore_str);
		}
		scores.close();		
		
	}
	//Display failure message if cannot find file
	else std::cout << "Error loading file!";
	ofstream newScore("scores.txt");
	if (newScore.is_open()) {
		if (curScore > highScore) {
			std::cout << "New high score!! Score: " << curScore << endl;
			std::cout << "Enter your initials ie. 'XXX': " << endl;
			std::cin >> initials;
			if (initials.length() < 3) {
				while (initials.length() < 3) {
					std::cout << "Please Enter 3 Characters for your initials" << endl;
					std::cin >> initials;
				}
			}
			newScore << initials << " : " << curScore <<  endl;
		}
		newScore.close();
	}	
	else std::cout << "Error loading file!";
}
//Display High Scores
void displayScore() {
	string line;
	ifstream scores("scores.txt"); //Text file in program location
	if (scores.is_open()) {

		//Read Text file, line by line
		while (getline(scores, line)) {

			std::cout << "\nCurrent High Score: " << line << endl;
			scores.close();

		}
	}
}
int main()
{
	srand(time(NULL)); //initialize time for random numbers
	int playerCount = 0;
	int winScore = 10000;
	bool win = false;
	int playerScore[100] = { 0 };
	int turnRolls[6];
	int reRoll[] = { 0 };

	//Display Rules
	ruleText();

	//Display Highscore
	displayScore();

	//Need two players to start
	while(playerCount < 2) {

		std::cout << "\nEnter number of players: " << endl;
		std::cin >> playerCount;
		if (playerCount < 2) {
			std::cout << "Two or more players are required, please add more players." << endl;
		}
	}
	std::cout << "Let's Play!" << endl;
	//Main game loop
	while (win == false) {

		//Player's Turn Loop
		for (int p = 1; p < playerCount+1; ++p) {
			std::cout << "****************************Player " << p << "'s turn!*****************************\n";
			int turnScore = 0;
			string enter;
			int scoredDie = 0;
			char numtoRoll;
			int totalScore = 0;
			//Carrying Scores over
			int prevPlayerScore = playerScore[p];
			//Rolling for the turn
			for (int turnR = 0; turnR < 6; ++turnR) {
				turnRolls[turnR] = roll(turnR);
			}				
			scoredDie = 0;				
			totalScore = score(turnRolls, scoredDie, 6);				
			turnScore = turnScore + totalScore;				
			int unScored = 6 - scoredDie;

			//FARKLE - Score 0
			if (unScored == 6) {
				std::cout << "Farkle!" << endl;
				std::cout << "Unfortunately to scored a Farkle, you scored 0 this turn." << endl;
				turnScore = 0;
				continue;
			}
			//Scored all 6 die, Roll Again!
			if (unScored == 0) {
				std::cout << "You scored on all 6, you will roll again!" << endl;
				scoredDie = 0;
				int firstScore = turnScore;
				for (int i = 0; i < 6; i++) {
					reRoll[i] = roll(i);
				}
				int extraRollScore = score(reRoll, scoredDie, 6);
				//Farkle on Re-roll.
				if (scoredDie == 0) {
					std::cout << "Farkle!" << endl;
					std::cout << "Unfortunately to scored a Farkle, you scored 0 this turn." << endl;
					p++;
					turnScore = 0;
					continue;
				}
				turnScore = firstScore + extraRollScore;
				std::cout << "Turn score: " << turnScore << endl;
				std::cout << "Player " << p << "'s score: " << playerScore[p] << endl;
				break;
			}				
								
			int reRollScore = 0;				
			unScored = 6 - scoredDie;
			cout << "Current turn score: " << turnScore << endl;

			//Re-Rolling unscored die
			cout << "You can re-roll " << unScored << " dice." << endl;
			cout << "Do you want to re-roll? Press n if you do not.\n";
			cin >> numtoRoll;
			while (numtoRoll != 'n' && numtoRoll != 'y' && numtoRoll != 'Y' && numtoRoll != 'N') {

				cout << "You have a maximum of " << unScored << " non-scoring dice to re-roll." << endl;
				cout << "Do you want to re-roll?" << endl;
				cin >> numtoRoll;
			}
			//Skipping the re-roll
			if (numtoRoll == 'n' | numtoRoll == 'N') {
				playerScore[p] = prevPlayerScore + turnScore;				
			}
			//Scoring the re-roll
			else {
				for (int i = 0; i < unScored; i++) {
					reRoll[i] = roll(i);
				}
				reRollScore = score(reRoll, scoredDie, unScored);
					

				std::cout << "Re-roll Score: " << reRollScore << endl;
				turnScore = turnScore + reRollScore;
				std::cout << "turnScore: " << turnScore << endl;					
			}							

			playerScore[p] = prevPlayerScore + turnScore;
			std::cout << "Player " << p <<"'s score: " << playerScore[p]<< endl;
			

			//Winning over 10,000p
			if (playerScore[p] > 10000) {
				int winScore = playerScore[p];
				std::cout << "Player " << p << " Wins!!" << endl;
				highScores(winScore);
				win = true;
				break;
			}		
		}
	}
}



```



# Database

### Code Review:

[![Alt text](https://img.youtube.com/vi/N_KMR_PlxH0/0.jpg)](https://www.youtube.com/watch?v=N_KMR_PlxH0)


   For the databases section of enhancements, I have chosen to enhance the MongoDB and RestAPI project from CS-340. I have chosen this project because it was definitely the most complex database project that I have completed here at SNHU. My SQL classes were a bit more basic and I thought MongoDB was interesting because how it is similar to SQL but how it takes up a notch in the query formation. Also, with the RestfulAPI with Python it is a combination of many classes that I have taken here at SNHU in one project.</p>
	
   The final for the class was broken up into many files that were to be submitted for each section of the final. So, only a few queries were in each file or not connected to the Rest API at all. With this enhancement I have chosen to combine the final into one final and have it all work within the Rest API. This involved changing a bit of how the coding worked, and adding routing for each of the methods so that they can be addressable through an URL address.
	
   While creating this enhancement, I was able to fix small mistakes and remove unneeded pieces of code that were left over from when I originally made it. The process also helps cement the ideas and methods of the Rest Api and how easy it can be to work with. I had a few struggles with creating it as a few methods had more than one input to the get the required information. This originally was handled within the command line and would ask for input. I was able to get through this with multiple variables within the URL and the methods pull those variables out without issue. I had to experiment a few times to make sure that this would work correctly but everything works smoothly.  This project will aid me in the future as it can related to a backend or full-stack developer position as Rest API experience in listed in a lot of positions, at least in my job market (near Boston). 


### Code:
```python
#!/usr/bin/python
import json
from bson import json_util
import bottle
from bottle import route, run, request, abort, post, get, put
import pymongo
from pymongo import MongoClient

##Local Connection
connection = MongoClient('localhost', 27017)

## DB and Collection
db = connection['market']
collection = db['stocks']


# set up URI paths for REST service
@route('/')
def index():
    cursor = collection.find().limit(10)
    print(list(cursor))

@route('/hello', method='GET')
def get_hello():
    try:
        request.query.name
        name = request.query.name

        if name:
            string = "{ \"hello, \"" + request.query.name + "\"}"
        else:
            string = "{ \"hello, World!\"}"

    except NameError:
        abort(404, 'No parameter for id')  # %s' % id)

    if not string:
        abort(404, 'No id')  # %s' % id)
    return json.loads(json.dumps(string, indent=4, default=json_util.default))


@route('/strings', method='POST')
def post_hello():
    try:

    string1 = request.json.get('string1')
    string2 = request.json.get('string2')

    if string1:
        string = "{ \"first word:, \"" + str(string1) + "\", second word:, \"" + str(string2) + "\"}"
    else:
        string = "{ \"Hello, World! Something is not right\"}"

    except NameError:
    abort(404, 'No parameter for id')  # %s' % id

    if not string:
        abort(404, 'No id')  # %s' % id)
    return json.loads(json.dumps(string, indent=4, default=json_util.default))


@route('/create', method='POST')
def post_create():
    data = request.body.readline()

    if not data:
        abort(400, 'No data received')

    entity = json.loads(data)

    if not entity.has_key('id'):
        abort(400, 'No id specified')

    try:

        collection.save(entity)

    except ValidationError as ve:

        abort(400, str(ve))

    print
    "Successfully saved"


###CREATE  
@route('/stocks/api/v1.0/createStock/<ticker>', method=['GET', 'POST'])
def createStock(ticker):
    try:
        ticdoc = collection.insert_one({"Ticker": ticker})
    except ValidationError as ve:
        abort(400, str(ve))

    print("Successfully Saved")


##READ
@route('/stocks/api/v1.0/getStock/<ticker>', method='GET')
def getStock(ticker):
    try:
        ticRead = collection.find({"Ticker": ticker})
    except ValidationError as ve:
        abort(400, str(ve))
    for x in ticRead:
        print(x)


##UPDATE
@route('/stocks/api/v1.0/updateStock/<ticker>/<newsector>', method=['PUT', 'GET'])
def put(ticker, newsector):
    try:
        ticdoc = collection.update_one({"Ticker": ticker}, {"$set": {"Sector": newsector}})
    except ValidationError as ve:
        abort(400, str(ve))

    print("Successfully Updated")


##DELETE
@route('/stocks/api/v1.0/deleteStock/<ticker>', method=['GET', 'DELETE'])
def getStock(ticker):
    try:
        ticRead = collection.delete_one({"Ticker": ticker})
    except ValidationError as ve:
        abort(400, str(ve))
    print("Successfully Deleted!")


##STOCK REPORT
@route('/stockReport', method='GET')
def get_stock():
    # global id
    # id = id + 1
    try:
        request.query.name
        name = request.query.name

        tickers = request.query.list
        print("**********Stock Summary for ticker: " + tickers + " **********")

        ticRead = collection.find({"Ticker": tickers},
                                  {"_id": 0,
                                   "Ticker": 1,
                                   "Company": 1,
                                   "Sector": 1,
                                   "Industry": 1,
                                   "Price": 1,
                                   "Volume": 1,
                                   "Shares Outstanding": 1,
                                   "Total Debt/Equity": 1})

        for x in ticRead:
            print(x)


    except NameError:
        abort(404, 'No parameter for id')  # %s' % id)


##TOP FIVE
@route('/stocks/api/v1.0/industryReport/<industry>', method='GET')
def topFive(industry):
    try:
        pipeline = [
            {"$match": {"Industry": industry}},
            {"$group": {"_id": "$Ticker", "Price": {"$max": "$Price"}}},
            {"$sort": {"Price": -1}},
            {"$limit": 5}
        ]

        query = db.stocks.aggregate(pipeline)

    except ValidationError as ve:
        abort(400, str(ve))
    for x in query:
        print(x)
        # Example: curl 'http://localhost:8080/stocks/api/v1.0/industryReport/Medical%20Laboratories%20%26%20Research'


##TICKERS
@route('/stocks/api/v1.0/industrytickers/<industry>', method='GET')
def tickers(industry):
    print("**********Industry Ticker Symbols**********")
    try:
        query = {"Industry": industry}
        tickers = collection.find({"Industry": industry}, {"Ticker": 1, "_id": 0})

    except NameError as ve:
        abort(400, str(ve))

    for x in tickers:
        print(x)


##SECTORS
@route('/stocks/api/v1.0/outstanding/<sector>', method='GET')
def sectorshares(sector):
    print("**********Total Outstanding Shares**********")
    try:
        pipeline = [
            {"$match": {"Sector": sector}},
            {"$group": {"_id": "$Sector", "total": {"$sum": "$Shares Outstanding"}}}
        ]

        query = db.stocks.aggregate(pipeline)


    except NameError as ve:
        abort(400, str(ve))

    for x in query:
        print(x)

    ##SIMPLE MOVING


@route('/stocks/api/v1.0/50day/<low>/<high>', method='GET')
def simplemoving(low, high):
    print("**********50-Day Simple Moving AVG**********")
    try:
        query = {"50-Day Simple Moving Average": {"$gt": low, "$lt": high}}
        count = collection.find({"50-Day Simple Moving Average": {"$gt": low, "$lt": high}}).count()

    except NameError as ve:
        abort(400, str(ve))

    print("Count of 50-Day Simple Moving Averages of: \n    "
          + "Greater than: " + str(low) + "\n    Less than: " + str(high) + "\n\n

    if __name__ == '__main__':
    # app.run(debug=True)
        run(host='localhost', port=8080)

```
# Design

### Code Review:

[![Alt text](https://img.youtube.com/vi/_BsOKr0SVdE/0.jpg)](https://www.youtube.com/watch?v=_BsOKr0SVdE)
The Zoo Authenticator project was a project about algorithms and mainly based on the MD5 Hashing. It was created for the IT-145 Software Development class. I have chosen this project as I feel that it as my first real feeling of success while writing code and really enjoyed the project. I chose this piece of coding work because it was one of the first real software classes, we were tasked to complete here at SNHU and wanted to improve upon that. The inclusion of MD5 Hashing was a nice introduction on data security and how these type of algorithms work. Though, MD5 now is not that secure, especially for projects that are on the internet. As MD5 encryption has been cracked, meaning that it can relatively easily be decrypted. The project gives insight on how you are supposed to handle passwords in terms of passing encrytped passwords for comparison instead of passing the un-encrypted string.

To improve the software, I wanted to expand upon the Zoo computer system to offer more options and display different things. Like each role within the software was supposed to be able to do different tasks when the logged in. I wanted to allow them to see these options and be able to access these screens. The Zookeeper to be able to see the daily logs like their role says they can, the admin be able to see the logged in users.

During the process of implementing the enhancements, I was trying to figure out where and what the best options were to do the tasks. Is it strictly a text based “adventure,” or should it be something else? I chose to make it more of a text-based project, with each role being able to see their options that they have be allowed to do and be able to access these pages. During the process, my data actually corrupted, and I had to load from a backup and continue working but I was able to recover the project. I liked the idea of having the zoo data computer system as operational and I think this makes it more fun to load up and run rather than just testing out the MD5 hashing function, a more text based simulation game. 

### Code:
```java

/**
 *
 * @author Zach Muse
 * IT145-1672
 * SNHU
 */
import java.util.Scanner;
import java.security.MessageDigest;

public class ZooAuthentication {

    /**
     * @param args the command line arguments
     */
    
    public static String md5Hash(String userPW) throws Exception {
                String original = userPW;  //Replace "password" with the actual password inputted by the user
		String md5PW ="";                
                MessageDigest md = MessageDigest.getInstance("MD5");
		md.update(original.getBytes());
		byte[] digest = md.digest();
                StringBuffer sb = new StringBuffer();
		for (byte b : digest) {
			sb.append(String.format("%02x", b & 0xff));
		}                
		md5PW = sb.toString();                
                
                return md5PW;
    }
    public static String askPW(){
        Scanner scnr = new Scanner(System.in);
        String userPW;
        System.out.println("Password: ");
        
        return userPW = scnr.nextLine();
    }
    public static int Attempts(int attemptNum){
        int userAttempts = attemptNum;
        
        userAttempts = attemptNum + 1;
                    System.out.println("Password is invalid. Try again, you have " + (3 - userAttempts) + " remaining");                    
                    return userAttempts;
    }   
    public static void printAdmin(){
        System.out.println("Hello, System Admin!\n" +
    "\n" + "As administrator, you have access to the zoo's main computer system.  This allows you "
         + "to monitor users in the system and their roles.");
    }
    public static void printVet(){
        System.out.println("Hello, Veterinarian!\n" +
    "\n" + "As veterinarian, you have access to all of the animals' health records. This allows you "
         + "to view each animal's medical history and current treatments/illnesses (if any), and to maintain a vaccination log.");
    }
    public static void printZooKeeper(){
        System.out.println("Hello, Zookeeper!\n" +
    "\n" + "As zookeeper, you have access to all of the animals' information and their daily monitoring logs. "
         + "This allows you to track their feeding habits, habitat conditions, and general welfare.");
        System.out.println("Please Select a Task:");
        System.out.println("1: Animal Information");
        System.out.println("2: Daily Logs");
    }
    public static void launchAnimals(){
        System.out.println("Hello, Zookeeper!\n" +
    "\n" + "Here is the animal information: \n");
        System.out.println("Current animal numbers: 23");
        System.out.println("Current sick animals: 0");
        System.out.println("Current healthy animals: 23");        
    }
    public static void dailyLogs(){
        System.out.println("Hello, Zookeeper!\n" +
    "\n" + "Here are the daily logs: \n");
        System.out.println("Elephants: ");
        System.out.println("Elephants were fed, given medicine, and washed. \n");
        System.out.println("Tigers: ");
        System.out.println("Lilly the tiger had a broken claw, it was clipped back and cleaned. ");
    }
    public static void systemLogs(){
        System.out.println("Hello, Admin!\n" +
    "\n" + "Here are the system logs: \n");
        System.out.println("Users onlne: 9");
        System.out.println("Vets: 3");
        System.out.println("Zookeepers: 3");
        System.out.println("Admins: 3");
    }
    public static void healthRecords(){
        System.out.println("Hello, Doctor!\n" +
    "\n" + "Here are the bealtb records: \n");
        System.out.println("Elephants: ");
        System.out.println("Elephant #2 was given medicine for foot \n");
        System.out.println("Tigers: ");
        System.out.println("Tiger #1 had a broken claw, it was clipped back and cleaned. "
        		+ "Tiger #3: sedated and performed a teeth cleaning");
    }
        
    public static void main(String[] args) throws Exception {
        
    Scanner scnr = new Scanner(System.in);
    String userPW = "";
    String userName= "";
    int userAttempts = 0;    
    String userInput;
    final String role1 = "Zookeeper";
    final String role2 = "Veterinarian";
    final String role3 = "Admin";
    final int maxUsers = 7;
    final String logOut = "log out";
        
    Boolean userAccess = false; /// Allows user to progress
    String md5PW;
    ///////////////////////////////USERS////////////////////
    String [] user = new String[7];
    user[0] = "griffin.keyes";
    user[1] = "rosario.dawson";            
    user[2] = "bernie.gorilla";
    user[3] = "donald.monkey";
    user[4] = "jerome.grizzlybear";
    user[5] = "bruce.grizzlybear";
    user[6] = "admin";
    ///////////////////////////////HASHED PW///////////////
    String [] hash = new String [7];
    hash[0] = "108de81c31bf9c622f76876b74e9285f";
    hash[1] = "3e34baa4ee2ff767af8c120a496742b5";
    hash[2] = "a584efafa8f9ea7fe5cf18442f32b07b";
    hash[3] = "17b1b7d8a706696ed220bc414f729ad3";
    hash[4] = "3adea92111e6307f8f2aae4721e77900";
    hash[5] = "0d107d09f5bbe40cade3de5c71e9e9b7";
    hash[6] = "21232F297A57A5A743894A0E4A801FC3";
    ///////////////////////////////ROLES////////////////////
    String [] role = new String [3];
    role[0] = "Zookeeper";
    role[1] = "Veterinarian";
    role[2] = "Admin";
    String currentRole = "";
    ///////////////////////////USERROLESS///////////////////
    String [] userRole = new String [7];
    userRole[0] = role[0];
    userRole[1] = role[2];
    userRole[2] = role[1];
    userRole[3] = role[0];
    userRole[4] = role[1];
    userRole[5] = role[2];
    userRole[6] = role[2];
    
    System.out.println("Username: ");
    userName = scnr.nextLine();
    userPW = askPW();
    md5PW = md5Hash(userPW);
    //while (userAccess != true) {
    while (userAttempts < 3 || userAccess == false) {               
        for (int i=0; i < 7; ++i){            
            if (userName.equalsIgnoreCase(user[i])){                
                while (userAccess == false) {                    
                    if (md5PW.equals(hash[i])){
                        System.out.println("Access granted.");                        
                        currentRole = userRole[i];                        
                        userAccess = true;
                        break;                                               
                    }
                    else {
                        userAttempts = Attempts(userAttempts);                                                                                    
                        userPW = askPW();
                        md5PW = md5Hash(userPW);
                        if (userAttempts == 3){
                            break;
                        }
                    }
                } 
            }
            else {
                if (userAccess == false){
                    System.out.println("Error - Username not found.");
                    System.out.println("Please contact System Administration to set up new accounts. If an error, please try again.");
                    userAttempts = 3;
                    break;
                }
            }
       }
       if (userAttempts == 3){
           System.out.println("System Exiting: Goodbye!");
           break;
       }
       else if (userAccess == true) {
           break;
       }       
    }
    if (userAccess == true) {                   ///Checks to make sure User has access
        System.out.println("Welcome " +userName + ", your role is " + currentRole + ".");
        System.out.println("");
        if (currentRole.equals(role[2])){
            printAdmin();
        }
        else if (currentRole.equals(role[1])){
            printVet();
        }
        else if (currentRole.equals(role[0])){
            printZooKeeper();
        }      
      }
    while (userAccess == true) { // User stays logged in unless typing "log out"
        System.out.println();
        System.out.println("Type \"log out\" to exit.");
        userInput = scnr.nextLine();
        if (userInput.equals(logOut)){
            userAccess = false;
            break;        
        }
        if (userInput.equals("1")) {
        	launchAnimals();       	
        }
        if (userInput.equals("2")) {
        	dailyLogs();
        }
    }
   }    
}
```


