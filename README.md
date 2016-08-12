# ngram-word-generator

Word generation based on n-gram models, and a cli utility to generate said models.

N-gram models (see on [wikipedia](https://en.wikipedia.org/wiki/N-gram)) are often used among other things to generate sentences and can produce satisfying results, though the inherent lack of context tracking and actual grammar checking is a severe shortcoming.
When used as a word generator this simple algorithm really shines at producing new, imaginary words. For example here are few names generated based French firstnames - *Mélaïde, Hadrigue, Laudence, Vestine, Pasien, Eulalisse* - and on Irish firstnames - *Ceallán, Muiríne, Toirdre, Finnbhach, Dáirtín, Éannailís*.

A previous version of the project can be seen in use in [Stochastèmes](http://www.kchapelier.com/stochastemes/), a word generator based on poetic corpora.

## Features

 - Works in node and in browsers
 - A cli utility to do the heavy-lifting of generating the n-gram models from text files
 - The generated models are standard json files, optimized for web transfer
 - Porting the client code to other languages should be trivial, the generated n-gram models can thus be used virtually anywhere

```js
var makeGenerator = require('ngram-word-generator'),
    ngramModel = require('./irish-firstnames.json'); //generated with the cli utility

var generator = makeGenerator(ngramModel);

console.log(generator(10));
```

## Public API

### makeGenerator(ngramModel)

Create a generator based on a given n-gram model.

**Options**

 - *ngramModel :* An n-gram model as generated by the cli utility.

### generator(lengthHint, rngFunction)

Generate a word based on the n-gram model.

**Options**

 - *lengthHint :* Suggest the desired word length. The nature of the algorithm makes it impossible to ensure the exact length of the result (except through brute-force).
 - *rngFunction :* Random number generator function. Default to Math.random.

## CLI

The cli utility is used to generate the n-gram models

`ngram-word-generator source.txt > model.json`

In Unix-like environment, the cli utility can also be piped into :

`cat source.txt | ngram-word-generator > model.json`

### ngram-word-generator sourceFile [options]

*sourceFile* must be an utf8 text file. No structure required.

**Options**

 - name: A name for the model, not directly used.
 - n: The order of the model (1: unigram, 2: bigram, 3: trigram, etc.). Default to 3.
 - minLength: The minimum length of the word included in the generation of the model. Default to 4.
 - unique: Usually if multiple instances of a specific word is included in the source file, the model will be skewed toward generating similar words. Setting this option ensure this doesn't happen.
 - compress: Reduce the size of the model file, making it less readable and slightly less precise.
 - excludeOriginal: The model will include the full list of the words included in the source file so that the generation can exclude them.
 - filter: Character filtering option. Default to extended.

**Filters**

 - none: All non-whitespace characters
 - alphabetical: A to Z
 - numerical: 0 to 9
 - alphaNumerical: A to Z and 0 to 9
 - extended: A to Z and éèëêęėēúüûùūçàáäâæãåāíïìîįīóöôòõœøōñńß
 - extendedNumerical: A to Z, 0 to 9 and éèëêęėēúüûùūçàáäâæãåāíïìîįīóöôòõœøōñńß

**Full example**

```
ngram-word-generator source.txt --n=3 --minLength=4 --filter=extended --compress --unique --excludeOriginal > model.json
```

## Changemap

### [1.0.0](https://github.com/kchapelier/ngram-word-generation/tree/1.0.0) (2016-08-12) :

 * First publication.

## Roadmap

 - Include more default filtering options
 - More flexible character filtering
 - Implement a dictionary generation function
 - Make it possible (and document how) to use the model generation outside of the cli utility
 - Make an online tool to generate the n-gram models

## License

MIT
