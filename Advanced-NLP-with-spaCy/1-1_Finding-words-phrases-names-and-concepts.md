# Introduction to spaCy

## The nlp object
```python
# Import the English language class
from spacy.lang.en import English
# Create the nlp object
nlp = English()
```

- contains the processing pipeline
- includes language-specific rules for tokenization etc.

## The Doc object
```python
# Created by processing a string of text with the nlp object
doc = nlp("Hello world!")
# Iterate over tokens in a Doc
for token in doc:
    print(token.text)
```

Token objects represent the tokens in a document. (i.e. word or punctuation character)

```python
doc = nlp("Hello world!")

# Index into the Doc to get a single Token
token = doc[1]
# Get the token text via the .text attribute
print(token.text)
```

## The Span object
The span object is a slice of the document consisting of one or more tokens.
It's only a view of the doc and doesn't contain any data itself.
To create a span, use Python slice notation:
```python
doc = nlp("Hello world!")

# A slice from the Doc is a Span object
span = doc[1:4]
# Get the span text via the .text attribute
print(span.text)
```

## Lexical attributes
```python
doc = nlp("it costs $5.")
# i is the index of the token within the doc
print('Index:	', [token.i for token in doc])
# text returns the token text
print('Text:	', [token.text for token in doc])
# is_alpha returns a boolean value indicating if the token consists of alphanumeric characters
print('is_alpha:', [token.is_alpha for token in doc])
# is_punct returns a boolean value whether the token is punctuation
print('is_punct:', [token.is_punct for token in doc])
# like_num returns a boolean value whether the token resembles a number
print('like_num:', [token.like_num for token in doc])
```
Lexical attributes refer to their entry in the vocabulary and don't depend on the token's context.

## Exercises
### Getting Started
1.
```python
# Import the English language class
from spacy.lang.en import English

# Create the nlp object
nlp = English()

# Prcoess a text
doc = nlp("This is a sentence.")

# Print the document text
print(doc.text)
```
2.
```python
# Import the German language class
from spacy.lang.de import German

# Create the nlp object
nlp = German()

# Prcoess a text (this is German for: "Kind regards!")
doc = nlp("Liebe GrüBe!")

# Print the document text
print(doc.text)
```
3.
```python
# Import the Spanish language class
from spacy.lang.es import Spanish

# Create the nlp object
nlp = Spanish()

# Process a text (this is Spanish for: "How are you?")
doc = nlp("¿Cómo estás?")

# Print the document text
print(doc.text)
```

### Documents, spans and tokens
1.
```python
# Import the English language class and create the nlp object
from spacy.lang.en import English
nlp = English()

# Process the text
doc = nlp("I like tree kangaroos and narwhals.")

# Select the first token
first_token = doc[0]

# Print the first token's text
print(first_token.text)
```
2.
```python
# Import the English language class and create the nlp object
from spacy.lang.en import English
nlp = English()

# Process the text
doc = nlp("I like tree kangaroos and narwhals.")

# A slice of the Doc for "tree kangaroos"
tree_kangaroos = doc[2:4]
print(tree_kangaroos.text)

# A slice of the Doc for "tree kangaroos and narwhals" (whtout the ".")
tree_kangaroos_and_narwhals = doc[2:6]
print(tree_kangaroos_and_narwhals.text)
```

### Lexical attributes
```python
# Process the text
doc = nlp("In 1990, more than 60% of people in East Asia were in extreme poverty. Now less than 4% are.")

# Iterate over the tokens in the doc
for token in doc:
    # Check if the token resembles a number
    if token.like_num:
        # Get the next token in the document
        next_token = doc[token.i+1]
        # Check if the next token's text equals '%'
        if next_token.text == '%':
            print('Percentage found:', token.text)
```
