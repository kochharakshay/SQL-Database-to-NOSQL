# SQL-Database-to-NOSQL

# Database: ICC Cricket World Cup 2019

# ABSTRACT:
In this assignment, we are converting a SQL relational database into a non-relational NoSQL database. We have used MongoDB to convert from relational to a non-relational database. This dataset contains all the tables related to the ICC Cricket world cup. Some tables have been previously scraped using Beautiful soup in Python. Additional tables have been web scraped which contain various tweets corresponding to the trending hashtags during the world cup.

# Libraries Required:
1. pymongo
2. tweepy
3. pandas
4. twitter

# MongoDb port: localhost:27017

# Datasets:
We had 3 tables that were normalized in the previous assignment. These tables were
1.	Players table
2.	Boards table
3.	Teams table

# Web scrape tweets from twitter
    CONSUMER_KEY = 'auTIVP4hHiJA7Snb4XYWWEofR'

    CONSUMER_SECRET = 'VeoKqZ1twMgYaxTSNB6MRO6dvg112BYuTajEZJMw8gzutjpDKR'

    ACCESS_TOKEN = '1121864797-GyTUHsApFDjZGoP4ovCDXudSq48wXUXGiBwVczo'

    ACCESS_TOKEN_SECRET = 'I5FK5vwn70xskQTgHema1VzAZLoHZIR1NDlTXHfvuTSVS'

    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)

    auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)

    api = tweepy.API(auth)

    #Create a new csv file

    csv_file = open('teamindia.csv', 'w+')

    #Write rows in those csv files

    csv_writer = csv.writer(csv_file)

    csv_writer.writerow(['Timestamp', 'Tweet', 'username', 'hashtags' ,"followers_count"])

    #Search for the mentioned hashtag

    for tweet in tweepy.Cursor(api.search, q="#teamindia",
                      lang="en",

                      since="2019-01-01").items(100):

        csv_writer.writerow([tweet.created_at, tweet.text.encode('utf-8'), tweet.user.screen_name.encode('utf-8'), [e['text'] for e in tweet._json['entities']['hashtags']] ,tweet.user.followers_count])

            
# API to scrape details of all players 
    CONSUMER_KEY = 'auTIVP4hHiJA7Snb4XYWWEofR'

    CONSUMER_SECRET = 'VeoKqZ1twMgYaxTSNB6MRO6dvg112BYuTajEZJMw8gzutjpDKR'

    OAUTH_TOKEN = '1121864797-GyTUHsApFDjZGoP4ovCDXudSq48wXUXGiBwVczo'

    OAUTH_TOKEN_SECRET = 'I5FK5vwn70xskQTgHema1VzAZLoHZIR1NDlTXHfvuTSVS'

    auth = twitter.oauth.OAuth(OAUTH_TOKEN, OAUTH_TOKEN_SECRET, CONSUMER_KEY, CONSUMER_SECRET)

    twitter_api = twitter.Twitter(auth=auth)

    print(twitter_api)

    auth = twitter.oauth.OAuth(OAUTH_TOKEN, OAUTH_TOKEN_SECRET, CONSUMER_KEY, CONSUMER_SECRET)

    twitter_api = twitter.Twitter(auth=auth)

    print(twitter_api)

    Top_ten_Test_Batsmen = ["@imVkohli", "@stevesmith49", "@marnus3cricket", "@NotNossy", "@davidwarner31",
                            "@cheteshwar1", "@babarazam258", "@ajinkyarahane88", "@root66", "@benstokes38"]

    player_name = []

    twitter_id = []

    followers = []

    friends = []

    verify = []

    #Using for loop to iterate and print all values

    for i in Top_ten_Test_Batsmen:

    twitter_id.append(i)

    player_name.append(twitter_api.statuses.user_timeline(screen_name=i)[1]['user']['name'])

    followers.append(twitter_api.statuses.user_timeline(screen_name=i)[1]['user']['followers_count'])

    friends.append(twitter_api.statuses.user_timeline(screen_name=i)[1]['user']['friends_count'])

    verify.append(twitter_api.statuses.user_timeline(screen_name=i)[1]['user']['verified'])

    Twitter_Specifications = pd.DataFrame({
                 'Player Name': player_name,
                 'Twitter ID': twitter_id,
                 'Followers Count': followers,
                 'Friends Count': friends,
                 'Verified': verify})

# Script to convert structured database to MongoDB
for index,row in df_players.iterrows():
    
    t_id = row['Team_Id']
    
    team = ""
    
    team_object = {}
    
    board_object = {}
    for index1,row1 in df_teams.iterrows():
        
        if row1['Team_ID'] == t_id:
            
            # team 
            team = row1
            team_object = {
            "t_id": team.Team_ID,
            "teamname" : team.Team_name
            }   
            
            # board
            b_id = team.Board_Id
            board = ""
            for index2,row3 in df_boards.iterrows():
                if row3['Board_Id'] == b_id:
                    board = row3

            board_object = {
                "Board Id": board.Board_Id,
                "Board Name": board['Board Name'],
                "Coach Name": board['Coach Name']
            }

    ICC = {
        "PlayerID" : row['Player_Id'],
        "Player" : row['Player_Name'],
        
        "team" : team_object,
        "cricket_board": board_object 
    }
    mycol.insert_one(ICC)
    
   # Read all csv files and insert into mongodb

    df_kohli_tweets = pd.read_csv("kohli.csv")

    df_benstokes_tweets = pd.read_csv("benstokes.csv")

    df_cwc19_tweets = pd.read_csv("cwc19.csv")

    df_cwc19final_tweets = pd.read_csv("cwc19final.csv")

    df_davidwarner_tweets = pd.read_csv("davidwarner.csv")

    df_rohitsharma_tweets = pd.read_csv("rohitsharma.csv")

    df_teamindia_tweets = pd.read_csv("teamindia.csv")

    df_teamaustralia_tweets = pd.read_csv("teamaustralia.csv")

    df_worldcup_tweets = pd.read_csv("worldcup.csv")

    df_most_runs = pd.read_csv("most_runs.csv", encoding = "ISO-8859-1")

    df_most_wickets = pd.read_csv("most_wickets.csv", encoding = "ISO-8859-1")

    #converting csv files to a data frame and inserting those data frame into mongodb 

    records1 = df_cwc19_tweets.to_dict(orient = 'records')

    result = mydb.cwc19_tweets.insert_many(records1 )

    records2 = df_cwc19final_tweets.to_dict(orient = 'records')

    result = mydb.cwc29final_tweets.insert_many(records2 )

    records3 = df_worldcup_tweets.to_dict(orient = 'records')

    result = mydb.worldcup_tweets.insert_many(records3 )

    records4 = df_teamindia_tweets.to_dict(orient = 'records')

    result = mydb.teamindia_tweets.insert_many(records4 )

    records5 = df_teamaustralia_tweets.to_dict(orient = 'records')

    result = mydb.teamaustralia_tweets.insert_many(records5 )

    records6 = df_kohli_tweets.to_dict(orient = 'records')

    result = mydb.kohli_tweets.insert_many(records6 )

    records7 = df_benstokes_tweets.to_dict(orient = 'records')

    result = mydb.benstokes_tweets.insert_many(records7 )

    records8 = df_rohitsharma_tweets.to_dict(orient = 'records')

    result = mydb.rohitsharma_tweets.insert_many(records8 )

    records9 = df_davidwarner_tweets.to_dict(orient = 'records')

    result = mydb.davidwarner_tweets.insert_many(records9 )

    records10 = Twitter_Specifications.to_dict(orient = 'records')

    result = mydb.Twitter_Specifications.insert_many(records10)

    records11 = df_most_runs.to_dict(orient = 'records')

    result = mydb.most_runs.insert_many(records11)

    records12 = df_most_wickets.to_dict(orient = 'records')

    result = mydb.most_wickets.insert_many(records12)

    records13 = df_teams.to_dict(orient = 'records')

    result = mydb.teams_dataset.insert_many(records13)

# Query to display the instance where players have social media account from ICC collection
    db.ICC.aggregate([{$lookup:{localField: "user_id", from : "Twitter_Specifications", foreignField: "_id", as: "TwitterHandle" }},{$project :{ "Player":1,"TwitterHandle.Twitter ID":1}}]).pretty();

# Use case which gives Highest Runs:
    db.most_runs.find().sort({Runs:-1}).limit(1).pretty()

# Use-cases which give most number of wickets:
    db.most_wickets.find().sort({Wickets:-1}).limit(1).pretty()

# Use-case which gives players with average runs scored
    db.most_runs.aggregate({$match : {Runs : { 𝑔𝑡:250,gt:250,lte : 350 }}}]).pretty()

# Use case to give highest trending player on twitter
    db.Twitter_Specifications.aggregate([{$sort:{"Followers Count" : -1}},{$project:{"_id": 0 ,"Player Name":1, "Followers Count":1}}]).pretty()

# Use-case to display team india players
    db.ICC.aggregate([{$match:{"team.teamname" :"India" }},{$project:{"_id":0,"PlayerID":1, "Player":1, "team.teamname":1}}]).pretty()

