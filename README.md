### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
    import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.document_ids = []
        self.matrix = None

    # Index all documents
    def index_documents(self, documents):
        self.document_ids = list(documents.keys())
        for doc_id, text in documents.items():
            for term in text.lower().split():
                self.index.setdefault(term, set()).add(doc_id)

    # Create document-term matrix
    def create_matrix(self, documents):
        terms = list(self.index.keys())
        self.matrix = np.zeros((len(documents), len(terms)), dtype=int)
        for i, (doc_id, text) in enumerate(documents.items()):
            for term in text.lower().split():
                if term in self.index:
                    self.matrix[i][terms.index(term)] = 1
        print("\n📊 Document-Term Matrix:\n")
        print(pd.DataFrame(self.matrix, index=self.document_ids, columns=terms))

    # Get 0/1 vector for a term
    def get_vector(self, term):
        terms = list(self.index.keys())
        return self.matrix[:, terms.index(term)] if term in terms else np.zeros(len(self.document_ids), dtype=int)

    # Boolean Search with match-case
    def search(self, query):
        parts = query.lower().split()
        result = np.ones(len(self.document_ids), dtype=int)
        i = 0
        while i < len(parts):
            part = parts[i]
            match part:
                case "and":
                    i += 1
                    continue
                case "or":
                    if i + 1 < len(parts):
                        result = np.logical_or(result, self.get_vector(parts[i + 1])).astype(int)
                        i += 2
                    else:
                        print("⚠️ OR must be followed by a term.")
                        return []
                case "not":
                    if i + 1 < len(parts):
                        result *= (1 - self.get_vector(parts[i + 1]))
                        i += 2
                    else:
                        print("⚠️ NOT must be followed by a term.")
                        return []
                case _:
                    result *= self.get_vector(part)
                    i += 1

        return [self.document_ids[j] for j, val in enumerate(result) if val == 1]
```
### Program2

```
    documents = {
    1: "Python is a programming language",
    2: "Information retrieval deals with finding information",
    3: "Boolean models are used in information retrieval"
}

br = BooleanRetrieval()
br.index_documents(documents)
br.create_matrix(documents)

query = input("\n🔎 Enter your Boolean query: ")
results = br.search(query)

if results:
    print(f"\n✅ Documents matching '{query}': {results}")
else:
    print(f"\n❌ No documents match '{query}'.")

```

### Output:

Information Retrieval Using Boolean Model in Python implemented successfully

<img width="957" height="387" alt="image" src="https://github.com/user-attachments/assets/acb400c5-514b-4375-bd76-e95d50e1feb7" />
<img width="986" height="435" alt="image" src="https://github.com/user-attachments/assets/34263f94-4059-464a-b4ee-55cd08689339" />



### Result:

Information Retrieval Using Boolean Model in Python implemented successfull
