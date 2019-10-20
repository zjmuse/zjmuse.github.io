### Welcome to Zach Muse's CS-499 ePortfolio!

 	### The Journey
  Even though I have been out of school for about 10 years and in a career not related to Computer Science, I decided I wanted to go back and improve myself and provide a better life for my family. Though this year and summer have been an extreme struggle for me, as I went through a house sale, a house rehabilitation, and a new baby born within all this time frame of finishing schooling.  The journey has so far taken close to 3 years now but managing school and having three children to provide for has taken most of my sanity. I have enjoyed the process and the classes I have taken and will be pursuing a career in Software Development.
	
  The github experience has proven beneficial as a lot of prospective careers are looking for some sort of source tree/github/bitbucket-type experience. 
# Algorithms
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
			//std::cout << line << '\n';
			//if (curScore == 0) {
			//	std::cout << "Current High Score: " << line << endl;
			//	scores.close();
			//	break;
			//}
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
	For the databases section of enhancements, I have chosen to enhance the MongoDB and RestAPI project from CS-340. I have chosen this project because it was definitely the most complex database project that I have completed here at SNHU. My SQL classes were a bit more basic and I thought MongoDB was interesting because how it is similar to SQL but how it takes up a notch in the query formation. Also, with the RestfulAPI with Python it is a combination of many classes that I have taken here at SNHU in one project.
	
	The final for the class was broken up into many files that were to be submitted for each section of the final. So, only a few queries were in each file or not connected to the Rest API at all. With this enhancement I have chosen to combine the final into one final and have it all work within the Rest API. This involved changing a bit of how the coding worked, and adding routing for each of the methods so that they can be addressable through an URL address. 
	
	While creating this enhancement, I was able to fix small mistakes and remove unneeded pieces of code that were left over from when I originally made it. The process also helps cement the ideas and methods of the RestApi and how easy it can be to work with. I had a few struggles with creating it as a few methods had more than one input to the get the required information. This originally was handled within the command line and would ask for input. I was able to get through this with multiple variables within the URL and the methods pull those variables out without issue. I had to experiment a few times to make sure that this would work correctly but everything works smoothly.  I plan to further comb through the code to make sure everything is working the best that it can and possibly add in some custom queries on top of the final queries. 


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


- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```


