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
