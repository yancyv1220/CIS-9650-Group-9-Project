import pandas as pd


# Read CSV file
f=open('movies.csv')
df = pd.read_csv(f)

print("Step 1")
print("====================================")
print(len(df),"records were read from file.")

print("Step 2")
print("====================================")
df['genre'] = df['genre'].str.split(', ')
df=df.explode('genre')
print(pd.unique(df["genre"]))
select=input("Please select genre:")
movies=df[df["genre"]==select.title()].sort_values("avg_vote",ascending=False)
for movie in movies["title"].head(100).values:
    print(movie)
movies.head(5).plot('title','avg_vote',kind="bar")



# Start of Number 4 (Yancy)


# Removing Rows that have Year in non 4-digit integer
mask = df.year.astype(str).str.len()<=4
df = df.loc[mask]

# Changing the data type of year to datetime
df['year'] = pd.to_datetime(df['year'], format='%Y')


# Filtering Dataframe to only include movies that contain the selected genre
gdf = df[df['genre'].str.contains(select)]


# Creating Unique set of years and intializing sets in preperation to plot line graph
yearset = sorted(set(df.year.unique()))
years = []
votes = []

# Looping to populate Empty Sets
for y in yearset:
    years.append(y)
    adf = gdf[(gdf['year'] == y)]
    length = len(adf)
    totalvote = adf['avg_vote'].sum()
    vote =  round((totalvote / length),2)
    votes.append(vote)

# Plotting the populated Sets
import matplotlib.pyplot as plt
plt.plot(years,votes)
plt.title('Average Rating Over the Years')
plt.xlabel('Year Published')
plt.ylabel('Average Rating')
plt.show()


# Presenting User with the choice of their time period
print("""\nPlease select the letter that corresponds with the time period that you want to filter by:\n
1 : 1920's or earlier
2 : In 1920's or 1930's
3 : In 1940's or 1950's
4 : In 1960's or 1970's
5 : In 1980's or 1990's
6 : In 2000's or later
""")

timeselect = int(input("> "))


# Defining the Dataframes filter based on selected time period
if timeselect == 1:
    sdf = gdf[(gdf['year'] < '1920-01-01')]
elif timeselect == 2:
    sdf = gdf[(gdf['year'] >= '1920-01-01') & (gdf['year'] < '1940-01-01')]
elif timeselect == 3:
    sdf = gdf[(gdf['year'] >= '1940-01-01') & (gdf['year'] < '1960-01-01')]
elif timeselect == 4:
    sdf = gdf[(gdf['year'] >= '1960-01-01') & (gdf['year'] < '1980-01-01')]
elif timeselect == 5:
    sdf = gdf[(gdf['year'] >= '1980-01-01') & (gdf['year'] < '2000-01-01')]
elif timeselect == 6:
    sdf = gdf[(gdf['year'] >= '2000-01-01')]


# Summarizing Rating and Duration of Movies in selected Time Period
length = len(sdf)
totalvote = sdf['avg_vote'].sum()
vote = round((totalvote / length),2)
totalduration = sdf['duration'].sum()
duration = (totalduration / length)

print(f"\nThere are {length} movies in this time period.")
print(f"The average rating for these movies are {vote} out of 10.")
print(f"The average duration for these movies are {duration} minutes.")


# Presenting Top 5 highest rated Movies in Time Period
Topsdf = sdf.sort_values('avg_vote', ascending = False).head(5)
print("\nOf these movies, these are the top 5 highest rated:\n")
print(Topsdf[['title','avg_vote']])
