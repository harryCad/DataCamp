# Statistical models

## What are statistical models?
Some of the most interesting things you can analyze are context specific.
For example, whether a word is a verb of whether a span of text is a person name.
- Enable spaCy to predict linguistic attributes *in context*
	- Part-of-speech tags
	- Synctactic dependencies
	- Named entities
- Trained on large datasets of labeled example texts
- Can be updated with more examples to fine-tune predictions

## Model Packages
spaCy provides a number of pre-trained model packages you can download.
i.e. "en\_core\_web\_sm" package is a small English model that supports all core capabilities and is trained on web text.
The spacy.load method loads a package by name and returns an nlp object.
```python
import spacy

nlp = spacy.load('en_core_web_sm')
```
- The package provides the binary weights that enable spaCy to make precdictions.
- Vocabulary
- Meta information (language, pipeline) to tell spaCy which language to use and how to configure the processing pipeline.

## Predicting Part-of-speech Tags
```python
import spacy

# Load the small English model
nlp = spacy.load('en_core-web_sm')
# Process a text
doc = nlp("She ate the pizza")
#Iterate over the tokens
for token in doc:
    # Print the text and the predicted part-of-speech tags
    print(token.text, token.pos_)
```
In spaCy, attributes that return strings usually end with an underscore.
Attributes without the underscore return an ID.

## Predicting Syntactic Dependencies
In addition to part-of-speech tags, we can also predict how the words are related.
i.e. whether a word is the subject of the sentence or an object.
The "dep underscore" attribute returns the predicted dependency label.
The head attribute returns the syntactic head token/parent token the word is attached to.
```python
for token in doc:
    print(token.text, token.pos_, token.dep_, token.head.text)
```

## Dependency label scheme
spaCy usses a standardized label scheme to describe syntactic dependencies.
- "She" is a nominal subject attached to the verg.
- Noun "pizza" is a direct object attached to the verb "ate".
- It is eated by the subject, "she".
- The determiner "the", the article, is attached to the noun "pizza".
![statistical-models_dependency-label-scheme](images/statistical-models_dependency-label-scheme.png)

## Predicting Named Entities
Named entities are "real world objects" that are assigned a name (i.e. a person, organization or a country).
The doc dot ents property lets you access named entities predicted by the model.
It returns an iterator of span objects so we can print the entity text and entity label using the "label underscore" attribute.
```python
# Process a text
doc = nlp(u"Apple is looking at buying U.K. startup for $1 billion")
#Iterate over the predicted entities
for ent in doc.ents:
    # Print the entity text and its label
    print(ent.text, ent.label_)
```

## Tip: the explain method
To get definitions for the most common tags and labels, you can use the spacy dot explain helper function.
i.e. "GPE" for geopolitical entity (countries, cities and states)
The same works for part-of-speech tags and dependency labels.
```python
spacy.explain('GPE')
spacy.explain('NNP')
spacy.explain('dobj')
```

## Exercises

### Loading models
1.
```python
# Load the 'en_core-web_sm' model - spaCy is already imported
nlp = spacy.load('en_core_web_sm')

text = "It's official: Apple is the first U.S. public company to reach a $1 trillion market value"

# Process the text
doc = nlp(text)

# Print the document text
print(doc.text)
```
2.
```python
# Load the 'de_core_news_sm' model – spaCy is already imported
nlp = spacy.load('de_core_news_sm')

text = "Als erstes Unternehmen der Börsengeschichte hat Apple einen Marktwert von einer Billion US-Dollar erreicht"

# Process the text
doc = nlp(text)

# Print the document text
print(doc.text)
```

### Predicting linguistic annotations
1.
```python
text = "It's official: Apple is the first U.S. public company to reach a $1 trillion market value"

# Process the text
doc = nlp(text)

for token in doc:
    # Get the token text, part-of-speech tag and dependency label
    token_text = token.text
    token_pos = token.pos_
    token_dep = token.dep_
    # This is for formatting only
    print('{:<12}{:<10}{:<10}'.format(token_text, token_pos, token_dep))
```
2.
```python
text = "It's offical: Apple is the first U.S. public company to reach a $1 trillion market value"

# Process the text
doc = nlp(text)

# Iterate over the predicted entities
for ent in doc.ents:
    # print the entity text and its label
    print(ent.text, ent.label_)
```

### Predicting named entities in context
```python
text = "New iPhone X release date leaked as Apple reveals pre-orders by mistake"

# Process the text
doc = nlp(text)

# Iterate over the entities
for ent in doc.ents:
    # print the entity text and label
    print(ent.text, ent.label_)

# Get the span for "iPhone X"
iphone_x = doc[1:3]

# Print the span text
print('Missing entity:' iphone_x.text)
```
