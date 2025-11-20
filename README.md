Practical 01: Implementation of CSV operations using Pandas
`import pandas as pd

# Create a simple DataFrame
data = {
    "Name": ["Alice", "Bob", "Charlie", "David"],
    "Age": [24, 27, 22, 32],
    "Marks": [85, 74, 91, 66],
}
df = pd.DataFrame(data)

print("=== Original DataFrame ===")
print(df)

# Show basic info
print("\n=== Info ===")
print(df.info())

# Statistical summary
print("\n=== Describe ===")
print(df.describe())

# Selecting a single column
print("\n=== Names column ===")
print(df["Name"])

# Filtering rows
print("\n=== Students with Marks > 80 ===")
print(df[df["Marks"] > 80])

# Sorting
print("\n=== Sorted by Marks (descending) ===")
print(df.sort_values(by="Marks", ascending=False))
`
Explanation: 
import pandas as pd
Loads pandas and gives it a short name pd (just a common shortcut).

pd.read_csv("data.csv")
Reads the CSV file and converts it to a DataFrame (table-like data structure: rows + columns).

df.head()
Shows the first 5 rows – good to quickly check data.

df.info()
Tells you:

how many rows

column names

data types (int, float, object/string)

how many non-null values.

df.describe()
For numeric columns: shows count, mean, std, min, max, quartiles.

df['col_1']
Selects one column as a Series (like one vector/column).

df[df['col_1'] > 0]
This is filtering rows: only keep rows where col_1 value is greater than 0.

Practical 02: CSV Read/write using pandas

`import pandas as pd

# 1. Create a small DataFrame manually
data = {
    "Name": ["Amit", "Bhavna", "Chetan"],
    "Marks": [85, 92, 76],
    "City": ["Vadodara", "Surat", "Ahmedabad"]
}

df = pd.DataFrame(data)
print("Original DataFrame:")
print(df)

# 2. Write this DataFrame to a CSV file
df.to_csv("students.csv", index=False)   # index=False avoids extra index column
print("\nData written to students.csv")

# 3. Read the CSV file back into another DataFrame
df2 = pd.read_csv("students.csv")
print("\nData read from students.csv:")
print(df2)
`
Practical 03 BFS and DFS Algorithm
`
from collections import deque

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}

def BFS(start):
    visited = set()
    queue = deque([start])

    print("BFS order:", end=" ")
    while queue:
        node = queue.popleft()
        if node not in visited:
            print(node, end=" ")
            visited.add(node)

            for neighbours in graph[node]:
                if neighbours not in visited:
                    queue.append(neighbours)

    print()

def DFS(node, visited=None):
    if visited is None:
        visited=set()
        print("DFS order:", end=" ")

    if node not in visited:
        print(node, end=" ")
        visited.add(node)

        for neighbour in graph[node]:
            DFS(neighbour, visited)

#Function calls
BFS('A')
DFS('A')
print()
`

Practical 4 – A* Algorithm
`import heapq  # for priority queue (always gives smallest element first)

# Graph: each node has a list of (neighbour, cost)
graph = {
    'S': [('A', 1), ('B', 4)],
    'A': [('C', 2)],
    'B': [('C', 1)],
    'C': [('G', 3)],
    'G': []
}

# Heuristic (h): guess of distance to goal G
h = {
    'S': 5,
    'A': 3,
    'B': 2,
    'C': 1,
    'G': 0
}

def a_star(start, goal):
    # (f, g, node, path)
    open_list = []
    heapq.heappush(open_list, (h[start], 0, start, [start]))
    closed = set()

    while open_list:
        f, g, node, path = heapq.heappop(open_list)

        if node in closed:
            continue

        if node == goal:
            print("A* Path:", " -> ".join(path))
            print("Total cost:", g)
            return

        closed.add(node)

        for neighbour, cost in graph[node]:
            if neighbour not in closed:
                new_g = g + cost
                new_f = new_g + h[neighbour]
                heapq.heappush(open_list, (new_f, new_g, neighbour, path + [neighbour]))

    print("No path found")

a_star('S', 'G')
`
Explanation (basic)

graph → tells which node goes to which neighbour and how costly.

h → heuristic = our guess of how far each node is from G.

open_list → nodes we still have to explore.

We store (f, g, node, path):

g = cost so far from start

h = guessed remaining cost

f = g + h = total estimate

heapq → always pops the smallest f (best node to explore).

We stop when node == goal and print path + cost.



Practical 5 – Fill Missing Values with Pandas
`
import pandas as pd
import numpy as np

data = {
    "Age": [20, 21, np.nan, 23, 24],
    "Marks": [85, np.nan, 75, 90, 88],
    "City": ["Vadodara", "Surat", "Surat", None, "Ahmedabad"]
}

df = pd.DataFrame(data)
print("Original:")
print(df)

# Check missing values
print("\nMissing values in each column:")
print(df.isnull().sum())

# Fill numeric columns with mean
df["Age"] = df["Age"].fillna(df["Age"].mean())
df["Marks"] = df["Marks"].fillna(df["Marks"].mean())

# Fill City (string) with most frequent value (mode)
df["City"] = df["City"].fillna(df["City"].mode()[0])

print("\nAfter filling missing values:")
print(df)
`

Explanation

np.nan / None → missing values.

df.isnull().sum() → how many missing values per column.

df["Age"].mean() → average Age (ignores missing).

fillna(...) → replaces missing values.

For City, we use mode() (most common value).



Practical 6 – k-Nearest Neighbour (k-NN) Classification

from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score

# Load dataset
iris = load_iris()
X = iris.data     # features
y = iris.target   # labels (0,1,2)

# Split into train and test sets (70% train, 30% test)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

# Create k-NN model with k = 3
knn = KNeighborsClassifier(n_neighbors=3)

# Train model
knn.fit(X_train, y_train)

# Predict on test data
y_pred = knn.predict(X_test)

# Evaluate
print("Accuracy:", accuracy_score(y_test, y_pred))

Explanation

load_iris() → famous ML dataset of flowers.

X = inputs (4 numbers per flower), y = which flower type.

train_test_split → separate data for training and testing.

KNeighborsClassifier(n_neighbors=3) → k = 3 nearest neighbours.

fit() → stores training data.

predict() → for each test point, find 3 nearest training points and take majority class.

accuracy_score() → how many predictions were correct.


Practical 7 – NLTK: Tokenization, Stemming, POS Tagging

`import nltk
from nltk.stem import PorterStemmer
from nltk.corpus import wordnet
from nltk import word_tokenize, pos_tag, NaiveBayesClassifier

# ------------------------------
# DOWNLOAD NEEDED PACKAGES
# ------------------------------
nltk.download('punkt')
nltk.download('punkt_tab')
nltk.download('averaged_perceptron_tagger_eng')
nltk.download('wordnet')

# ------------------------------
# SAMPLE TEXT
# ------------------------------
text = "Natural Language Processing helps computers understand human language."
print("\nOriginal Text:\n", text)

# ------------------------------
# 1. TOKENIZATION
# ------------------------------
tokens = word_tokenize(text)
print("\nTokens:\n", tokens)

# ------------------------------
# 2. STEMMING
# ------------------------------
stemmer = PorterStemmer()
stems = [stemmer.stem(t) for t in tokens]
print("\nStems:\n", stems)

# ------------------------------
# 3. POS TAGGING
# ------------------------------
tags = pos_tag(tokens)
print("\nPOS Tags:\n", tags)

# ------------------------------
# 4. SEMANTIC REASONING USING WORDNET
# ------------------------------
synonyms = set()
for syn in wordnet.synsets("good"):
    for lemma in syn.lemmas():
        synonyms.add(lemma.name())

print("\nWordNet synonyms for 'good': (first 10)\n", list(synonyms)[:10])

# ------------------------------
# 5. SIMPLE TEXT CLASSIFICATION (Naive Bayes)
# ------------------------------
def features(words):
    return {word: True for word in words}

# Training dataset
training_data = [
    (features(["love", "this", "movie"]), "positive"),
    (features(["hate", "this", "movie"]), "negative"),
    (features(["awesome", "acting"]), "positive"),
    (features(["terrible", "plot"]), "negative"),
]

classifier = NaiveBayesClassifier.train(training_data)

# Test sentence
test_sentence = "I love this awesome movie"
test_features = features(test_sentence.split())

print("\nTest Sentence:", test_sentence)
print("Predicted Class:", classifier.classify(test_features))
`

Explanation

word_tokenize(text) → split sentence into words/symbols.

PorterStemmer() → reduces words to root (e.g., “understanding” → “understand”ish).

pos_tag(tokens) → gives each word a grammatical tag:

NN = noun, VB = verb, etc.

This shows basic NLTK operations: tokenize → stem → tag.


Practical 8 – Support Vector Machine (SVM) Classification
`from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score

# Load data
iris = load_iris()
X = iris.data
y = iris.target

# Train/test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

# Create SVM model (RBF kernel)
svm_model = SVC(kernel='rbf')

# Train
svm_model.fit(X_train, y_train)

# Predict
y_pred = svm_model.predict(X_test)

# Evaluate
print("Accuracy:", accuracy_score(y_test, y_pred))
`

Explanation

Very similar structure to k-NN practical.

SVC(kernel='rbf') → SVM classifier with RBF kernel (handles non-linear boundaries).

fit() → learns best separating hyperplane between classes.

predict() → assigns class to each test sample.

accuracy_score() → performance measure.


Practical 9 – EM Algorithm (Gaussian Mixture Model)
`import numpy as np
from sklearn.mixture import GaussianMixture

# Create sample data (two clusters)
np.random.seed(0)
cluster1 = np.random.normal(loc=[0, 0], scale=1.0, size=(50, 2))
cluster2 = np.random.normal(loc=[5, 5], scale=1.0, size=(50, 2))
X = np.vstack((cluster1, cluster2))

# Create Gaussian Mixture Model with 2 components
gmm = GaussianMixture(n_components=2, random_state=0)

# Fit model (runs EM algorithm)
gmm.fit(X)

# Predict cluster labels
labels = gmm.predict(X)

print("Estimated cluster means:")
print(gmm.means_)
print("\nFirst 10 labels:")
print(labels[:10])
`

Explanation

Generate 2D points around [0,0] and [5,5] → two clusters.

GaussianMixture(n_components=2) → we assume 2 groups.

fit(X) → EM algorithm:

E-step: calculate probability of each point belonging to each cluster.

M-step: update cluster means/variances.

gmm.means_ → learned centers.

predict(X) → assign each point to a cluster (0 or 1).


Practical 10 – AI Search Problem (Water Jug with BFS)

from collections import deque

MAX4 = 4
MAX3 = 3

# state = (amount_in_4L, amount_in_3L)
def get_next_states(x, y):
    states = []

    # Fill jug 4
    states.append((MAX4, y))
    # Fill jug 3
    states.append((x, MAX3))
    # Empty jug 4
    states.append((0, y))
    # Empty jug 3
    states.append((x, 0))
    # Pour 4 -> 3
    pour = min(x, MAX3 - y)
    states.append((x - pour, y + pour))
    # Pour 3 -> 4
    pour = min(y, MAX4 - x)
    states.append((x + pour, y - pour))

    return states

def bfs_water_jug(start, goal):
    queue = deque()
    queue.append((start, [start]))  # (state, path)
    visited = set()

    while queue:
        (x, y), path = queue.popleft()

        if (x, y) in visited:
            continue

        if x == goal:
            print("Solution path:")
            for s in path:
                print(s)
            return

        visited.add((x, y))

        for nx, ny in get_next_states(x, y):
            if (nx, ny) not in visited:
                queue.append(((nx, ny), path + [(nx, ny)]))

    print("No solution found")

bfs_water_jug((0, 0), 2)

Explanation

State = (amount in 4L jug, amount in 3L jug).

get_next_states returns all valid next states from current state:

fill, empty, pour between jugs.

bfs_water_jug uses BFS to explore states until 4L jug has 2L (x == goal).

path keeps track of each step; we print it when we reach the solution.


Practical 11 – Neural Network–Based Application (XOR with sklearn)

No TensorFlow needed; this is enough for your exam.

from sklearn.neural_network import MLPClassifier
import numpy as np

# XOR input and output
X = np.array([
    [0, 0],
    [0, 1],
    [1, 0],
    [1, 1]
])

y = np.array([0, 1, 1, 0])

# Create a simple neural network
model = MLPClassifier(
    hidden_layer_sizes=(4,),  # 1 hidden layer with 4 neurons
    activation='relu',
    max_iter=5000,
    random_state=0
)

# Train the model
model.fit(X, y)

# Test on the same inputs
predictions = model.predict(X)
print("Predictions:", predictions)

🧠 Explanation

XOR problem:

0 0 → 0

0 1 → 1

1 0 → 1

1 1 → 0

MLPClassifier = Multi-Layer Perceptron (a basic neural network).

hidden_layer_sizes=(4,) → 1 hidden layer with 4 neurons.

activation='relu' → activation function for hidden layer.

fit(X, y) → trains the network.

predict(X) → NN tries to reproduce XOR mapping.
