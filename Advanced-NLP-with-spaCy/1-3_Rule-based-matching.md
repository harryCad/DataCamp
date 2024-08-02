# Rule-based matching

## Why not just regular expressions?
- Match on Doc objects, not just strings
- More flexible; match on tokens and token attributes
- Use the model's predictions
- Example: "duck" (verb") vs. "duck" (noun)

## Match patterns
- List of dictionaries, one per token
- Match exact token texts
```python
[{'ORTH': 'iPhone'}, [{'ORTH': 'X'}]
```
- Match lexical attributes
```python
[{'LOWER': 'iphone'}, {'LOWER': 'x'}]
```
- Match any token attributes
```python
[{'LEMMA': 'buy'}, {'POS': 'NOUN'}]
```

## Using the Matcher
- ```match_id```: has value of the pattern name
- ```start```: start index of matched span
- ```end```: end index of matched span

```python
import spacy

# Import the Matcher
from spacy.matcher import Matcher
# Load a model and create the nlp object
nlp = spacy.load('en_core_web_sm')
# Initialize the matcher with the shared vocab
matcher = Matcher(nlp.vocab)
# Add the pattern to the matcher
pattern = [{'ORTH': 'iPhone'}, ['ORTH': 'X'}]
matcher.add('IPHONE_PATTERN', None, pattern)
# Process some text
doc = nlp("New iPhone X release date leaked")
# Call the matcher on the doc
matches = matcher(doc)

#
#
#

# Call the matcher on the doc
doc = nlp("New iPhone X release date leaked")
matches = matcher(doc)
# Iterate over the matches
for match_id, start, end in matches:
    # Get the matched span
    matched_span = doc[start:end]
    print(matched_span.text)
```


## Matching lexical attributes
```python
pattern = [
    {'IS_DIGIT': True},
    {'LOWER': 'fifa'},
    {'LOWER': 'world'},
    {'LOWER': 'cup'},
    {'IS_PUNCT': True}
]
```
```python
doc = nlp("2018 FIFA World Cup: France won!")
```

## Matching other token attributes
```python
patter = [
    {'LEMMA': 'love', 'POS': 'VERB'},
    {'POS': 'NOUN'}
```
```python
doc = nlp("I loved dogs but now I love cats more.")
```

## Using operators and quantifiers
Operators and quantifiers let you define how often a token should be matched.
They can be added using the "OP" key.
The "?" operator makes the determiner token optional
```python
pattern = [
    {'LEMMA': 'buy'},
    {'POS': 'DET', 'OP': '?'},  # optonal: match 0 or 1 times
    {'POS': 'NOUN'}
```
```python
doc = nlp("I bought a smartphone. Now I'm buying apps.")
```
```shell
bought a smartphone
buying apps
```

"OP" can have one of four values:
- ```{'OP': '!'}``` Negation: match 0 times
- ```{'OP': '?'}``` Optional: match 0 or 1 times
- ```{'OP': '+'}``` Match 1 or more times
- ```{'OP': '*'}``` Match 0 or more times

## Exercises
