import pandas as pd


# Read CSV file
f=open('movies.csv')
df = pd.read_csv(f)

print("Step 1")
print("====================================")
print(len(df),"records were read from file.")

print("Step 2")
print("====================================")
input=input("Please select genre:")
movies = df.groupby("genre")['title'].count()
print(movies.nlargest)
