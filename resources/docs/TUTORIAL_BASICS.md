# Tutorial 1: NLP Base Types

This is part 1 of the tutorial, in which we look into some of the base types used in this library.

## Creating a Sentence

There are two types of objects that are central to this library, namely the `Sentence` and `Token` objects. A `Sentence` 
holds a textual sentence and is essentially a list of `Token`.

Let's start by making a `Sentence` object for an example sentence.

```python
# The sentence objects holds a sentence that we may want to embed
from flair.data import Sentence

# Make a sentence object by passing a whitespace tokenized string
sentence = Sentence('The grass is green .')

# Print the object to see what's in there
print(sentence)
```

This should print: 

```console
Sentence: "The grass is green ." - 5 Tokens
```

The print-out tells us that the sentence consists of 5 tokens. 
You can access the tokens of a sentence via their token id:

```python
print(sentence[4])
```

which should print 

```console
Token: 4 green
```

This print-out includes the token id (4) and the lexical value of the token ("green"). You can also iterate over all 
tokens in a sentence.

```python
for token in sentence:
    print(token) 
```

This should print: 

```console
Token: 1 The
Token: 2 grass
Token: 3 is
Token: 4 green
Token: 5 .
```

## Tokenization

In some use cases, you might not have your text already tokenized. For this case, we added a simple tokenizer using the
lightweight segtok library. 

Simply use the 'use_tokenizer' flag when instantiating your `Sentence` with an untokenized string:

```python
# The sentence objects holds a sentence that we may want to embed
from flair.data import Sentence

# Make a sentence object by passing an untokenized string and the 'use_tokenizer' flag
sentence = Sentence('The grass is green.', use_tokenizer=True)

# Print the object to see what's in there
print(sentence)
```

This should print: 

```console
Sentence: "The grass is green ." - 5 Tokens
```

## Adding Tags to Tokens

A Token has fields for linguistic annotation, such as lemmas, part-of-speech tags or named entity tags. You can 
add a tag by specifying the tag type and the tag value. In this example, we're adding an NER tag of type 'color' to 
the word 'green'. This means that we've tagged this word as an entity of type color.

```python
# add a tag to a word in the sentence
sentence[4].add_tag('ner', 'color')

# print the sentence with all tags of this type
print(sentence.to_tagged_string())
```

This should print: 

```console
The grass is green <color> .
```


## Reading CoNLL parsed files

We provide a set of helper methods to read CoNLL parsed files as a list of `Sentence` objects. For instance, you can
use the popular CoNLL-U format introduced by the Universal Dependencies project. 

Simply point the `NLPTaskDataFetcher` to the file containing the parsed sentences. It will read the sentences into a 
list of `Sentence`

```python
import NLPTaskDataFetcher

# use your own data path
data_folder = 'path/to/conll/formatted/data'

# get training, test and dev data
sentences: List[Sentence] = NLPTaskDataFetcher.read_conll_ud(data_folder)
```

Importantly, these sentences now contain a wealth of `Token` level annotations.
In the case of CoNLL-U, they should contain information including a token lemma, its part-of-speech, morphological annotation, its dependency relation and its head token.
You can access this information using the tag fields of the  `Token`.



## Next 

Now, let us look at how to use [pre-trained models](/resources/docs/TUTORIAL_TAGGING.md) to tag your text.
