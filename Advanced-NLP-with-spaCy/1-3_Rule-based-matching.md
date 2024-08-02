# Rule-based matching
- Match on Doc objects, not just strings
- Match on tokens and token attributes
- Use the model's predictions
- Example: "duck" (verb") vs. "duck" (noun)

## Rule-based Matching
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

## Why not just regular expressions?


## Match patterns


## Using the Matcher (1)


## Using the Matcher (2)


## Matching lexical attributes


## Matching other token attributes


## Using operators and quantifiers (1)


## Using operators and quantifiers (2)

