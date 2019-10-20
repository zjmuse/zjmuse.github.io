### Welcome to Zach Muse's CS-499 ePortfolio!

 	### The Journey
  Even though I have been out of school for about 10 years and in a career not related to Computer Science, I decided I wanted to go back and improve myself and provide a better life for my family. Though this year and summer have been an extreme struggle for me, as I went through a house sale, a house rehabilitation, and a new baby born within all this time frame of finishing schooling.  The journey has so far taken close to 3 years now but managing school and having three children to provide for has taken most of my sanity. I have enjoyed the process and the classes I have taken and will be pursuing a career in Software Development.
	
  The github experience has proven beneficial as a lot of prospective careers are looking for some sort of source tree/github/bitbucket-type experience. 

.

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Database
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


