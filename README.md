# Pre-training For Language-Understanding
Generalized Pre-training for Language Understanding 

## Overview
### Language Modeling
A language model captures **the distribution over all possible sentences**.
- Input : a sentence
- Output : the probability of the input sentence

It is _unsupervised learning_. In this repo, we turn this into a _sequence of supervised learning_.

## Build Corpus (Wikipedia)
Wikipedia regularly distributes the entire document. You can download Korean Wikipedia dump [here](https://dumps.wikimedia.org/kowiki/) (and English Wikipedia dump [here](https://dumps.wikimedia.org/enwiki/)).
Wikipedia recommends using `pages-articles.xml.bz2`, which includes only the latest version of the entire document, and is approximately 600 MB compressed (for English, `pages-articles-multistream.xml.bz2`).

You can use `wikipedia_ko.sh` script to download the dump on the latest Korean Wikipedia document. For English, use `wikipedia_en.sh`

example:
```
$ cd build_corpus
$ chmod 777 wikipedia_ko.sh
$ ./wikipedia_ko.sh
```

The downloaded dump using above shell script is in XML format, and we need to parse XML to text file. The Python script `WikiExtractor.py` in [attardi/wikiextractor](https://github.com/attardi/wikiextractor) repo, extracts and cleans text from the dump.

example:
```
$ git clone https://github.com/attardi/wikiextractor
$ python wikiextractor/WikiExtractor.py kowiki-latest-pages-articles.xml

$ head -n 4 text/AA/wiki_02
>> <doc id="577" url="https://ko.wikipedia.org/wiki?curid=577" title="천문학">
>> 천문학
>>
>> 천문학(天文學, )은 별이나 행성, 혜성, 은하와 같은 천체와, 지구 대기의 ..
>> ...
>> </doc>
```

The extracted text is saved as text file of a certain size. To combine these, use `build_corpus.py`. The output `corpus.txt` contains _4,277,241 sentences, 55,568,030 words_.

example:
```
$ python build_corpus.py > corpus.txt
$ wc corpus.txt 
>> 4277241  55568030 596460787 corpus.txt
```

## Preprocessing

### Build Vocab

example:
```
$ python build_vocab.py --corpus build_corpus/corpus.txt --vocab vocab.pkl
```

example:
```
$ python tokenizer.py --corpus build_corpus/corpus.txt --tokenizer mecab > build_corpus/tokenized_corpus.txt
$ wc build_corpus/tokenized_corpus.txt 
>> 4277241 124065001 664959628 build_corpus/tokenized_corpus.txt
```


## Reference
- [attardi/wikiextractor] [WikiExtractor](https://github.com/attardi/wikiextractor)