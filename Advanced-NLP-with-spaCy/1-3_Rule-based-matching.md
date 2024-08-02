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
[{'ORTH': 'iPhone'}, {'ORTH': 'X'}]
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
pattern = [
    {'LEMMA': 'love', 'POS': 'VERB'},
    {'POS': 'NOUN'}
]
```
```python
doc = nlp("I loved dogs but now I love cats more.")
```
```shell
loved dogs
love cats
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

### Using the Matcher
```python
# Import the Matcher and initialize it with the shared vocabulary
from spacy.matcher import Matcher
matcher = Matcher(nlp.vocab)

# Create a pattern matching two tokens: "iPhone" and "X"
pattern = [{'TEXT': 'iPhone'}, {'TEXT': 'X'}]

# Add the pattern to the matcher
matcher.add('IPHONE_X_PATTERN', None, pattern)

# User the matcher on the doc
matches = matcher(doc)
print('Matches:', [doc[start:end].text for match_id, start, end in matches])
```

### Writing match patterns
1.
```python
doc = nlp("After making the iOS update you won't notice a radical system-wide redesign: nothing like the aesthetic upheaval we got with iOS 7. Most of iOS 11's furniture remains the same as in iOS 10. But you will discover some tweaks once you delve a little deeper.")

# Write a pattern for full iOS versions ("iOS 7", "iOS 11", "iOS 10")
pattern = [{'TEXT': 'iOS', {'IS_DIGIT': True}]

# Add the pattern to the matcher and apply the matcher to the doc
matcher.add('IOS_VERSION_PATTERN', None, pattern)
matches = matcher(doc)
print('Total matches found:', len(matches))

# Iterate over the matches and print the span text
for match_id, start, end in matches:
    print('Match found:', doc[start:end].text)
```

2.
```python
doc = nlp("i downloaded Fortnite on my laptop and can't open the game at all. Help? so when I was downloading Minecraft, I got the Windows version where it is the '.zip' folder and I used the default program to unpack it... do I also need to download Winzip?")

# Write a pattern that matches a form of "download" plus proper noun
pattern = [{'LEMMA': 'download'}, {'POS': 'PROPN'}]

# Add the pattern to the matcher and apply the matcher to the doc
matcher.add('DOWNLOAD_THINGS_PATTERN', None, pattern)
matches = matcher(doc)
print('Total matches found:', len(matches))

# Iterate over the matches and print the span text
for match_id, start, end in matches:
    print('Match found:', doc[start:end].text)
```

3.
```python
doc = nlp("Features of the app include a beautiful design, smart search, automatic labels and optional voice responses.")

# Write a pattern for adjective plus one or two nouns
pattern = [{'POS': 'ADJ'}, {'POS': 'NOUN'}, {'POS': 'NOUN', 'OP': '?'}]

# Add the pattern to the matcher and apply the matcher to the doc
matcher.add('ADJ_NOUN_PATTERN', None, pattern)
matches = matcher(doc)
print('Total matches found:', len(matches))

# Iterate over the matches and print the span text
for match_id, starte, end in matches:
    print('Match found:', doc[starte:end].text)
```
