# Twitter Followers Analysis
Exploring the network of Twitter connections to uncover hidden patterns and key influencers using Pandas and NetworkX.

## Project Overview
Jump into the buzzing world of Twitter! In this project, you'll explore the fascinating network of Twitter connections and uncover the hidden patterns within one of the most popular social networks out there. You'll get hands-on with real Twitter follower data using Pandas, transforming it into a cool directed graph with NetworkX. Along the way, you'll learn how to spot key influencers, find out who follows who, and discover mutual connections between users.

## The Data
`twitter-followers.csv`
- **FOLLOWER**: ID of the user who is following
- **FOLLOWEE**: ID of the user being followed

## Files Included
- `twitter-followers.csv`: The dataset containing follower and followee IDs.
- `twitter_followers_analysis.ipynb`: The Jupyter Notebook with the code for data analysis.

## Analysis Steps
1. **Load CSV Data**: Using Pandas to read the CSV file and display the data.
2. **Create Directed Graph**: Using NetworkX to create a directed graph from the data.
3. **Functions Implemented**:
    - `is_following`: Checks if one user is following another.
    - `get_users_following_min_accounts`: Returns a list of user IDs following at least a specified number of accounts.
    - `get_mutual_followees`: Finds mutual followees between two users.
    - `get_most_connected_user`: Identifies the user with the most total connections (followers + followees).

## Example Usage 
### Imports you'll need for the project
```python
import pandas as pd
import networkx as nx

# Load csv data and store as edgelist (a directed graph)
df = pd.read_csv('twitter-followers.csv')
T = nx.from_pandas_edgelist(df, 'FOLLOWER', 'FOLLOWEE', create_using=nx.DiGraph())

# Display head of the DataFrame
print(df.head())
```

### Checking if User 1 is Following User 2
```python 
# The function is_following checks if user_id2 is following user_id1 in the directed graph T.
def is_following(T, user_id1, user_id2):
    # Check if there is a directed edge(connection) from user_id1 to user_id2 in the graph T.
    # In a directed graph, an edge(connection) from user_id1 to user_id2 means user_id1 is following user_id2.
    if T.has_edge(user_id2, user_id1):
        return True  # If the edge exists, return True indicating user_id1 is following user_id2.
    else:
        return False  # If the edge does not exist, return False indicating user_id1 is not following user_id2.
```

### Returns a list of user IDs who are following at least min_following_count
```python
def get_users_following_min_accounts(T, min_following_count):
 # Parameters:
 #    - T: Directed graph (e.g., from NetworkX)
 #    - min_following_count: Minimum number of accounts a user must be following
    users = []
    for user in T.nodes: #each user(node)
        following_count = len(list(T.successors(user)))  # Number of users this user is following(count of following)
        if following_count >= min_following_count:
            users.append(user)
    
    return users
#For this result meaning we want to find users who are following at least 500 accounts.
result = get_users_following_min_accounts(T, 500)
print(result)
```

### Finding Mutual Followees Between User 1 and User 27
```python
def get_mutual_followees(T, user_id1, user_id2):
# Parameters:
#     - T: Directed graph (e.g., from NetworkX)
#     - user_id1: ID of the first user
#     - user_id2: ID of the second user
# Get the set of followees for each user. 
# A set is used because it automatically handles duplicate entries and allows for easy set operations like intersection.
    followees1 = set(T.successors(user_id1))
    followees2 = set(T.successors(user_id2))
    
# Find the intersection (common followees)
    mutual_followees = followees1.intersection(followees2)
    
    return list(mutual_followees)

mutual_followees = get_mutual_followees(T, 1, 27)
print(mutual_followees)
```

### Identifying the Most Connected User
```python
def get_most_connected_user(T):
    max_connections = 0
    most_connected_user = None

    for user in T.nodes:
        num_followers = len(list(T.predecessors(user)))  # Number of followers
        num_followees = len(list(T.successors(user)))    # Number of followees
        total_connections = num_followers + num_followees
        
        if total_connections > max_connections:
            max_connections = total_connections
            most_connected_user = user
    
    return most_connected_user

most_connected_user = get_most_connected_user(T)
print(most_connected_user). 
```
## Conclusion
Throughout this project, we manipulated and organized Twitter follower data to extract meaningful insights, identifying key influencers and mutual connections within the network.




