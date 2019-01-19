[![Version](https://img.shields.io/badge/version-0.1.0.3-brightgreen.svg)
[![Python 3](https://img.shields.io/badge/python-3-blue.svg)](https://www.python.org/downloads/)

# NLP-Cube

NLP-Cube is an opensource Natural Language Processing Framework with support for languages which are included in the [UD Treebanks](http://universaldependencies.org/). Use NLP-Cube if you need:
* Sentence segmentation
* Tokenization
* POS Tagging (both language independent (UPOSes) and language dependent (XPOSes and ATTRs))
* Lemmatization
* Dependency parsing

Example input: **"This is a test."**, output is (in [Conll](https://universaldependencies.org/format.html) format): 
```
1       This    this    PRON    DT      Number=Sing|PronType=Dem        4       nsubj   _
2       is      be      AUX     VBZ     Mood=Ind|Number=Sing|Person=3|Tense=Pres|VerbForm=Fin   4       cop     _
3       a       a       DET     DT      Definite=Ind|PronType=Art       4       det     _
4       test    test    NOUN    NN      Number=Sing     0       root    SpaceAfter=No
5       .       .       PUNCT   .       _       4       punct   SpaceAfter=No
```

**To run NLP-Cube**, here's how to set up and use in a few lines: [Quick Start Tutorial](examples/1.%20NLP-Cube%20Quick%20Tutorial.ipynb).

For **advanced users that want to train their own models**, please the the Advanced Tutorials in ``examples/``, starting with how to [locally install NLP-Cube](examples/2.%20Advanced%20usage%20-%20NLP-Cube%20local%20installation.ipynb).

## Installation

Install (or update) NLP-Cube with:

```bash
pip install -U nlpcube
```
## Usage 

To use NLP-Cube **programmatically** (in Python), follow [this tutorial](examples/1.%20NLP-Cube%20Quick%20Tutorial.ipynb)
Here's a quick look on how to process a text:
```python
from cube.api import Cube       # import namespace
cube=Cube()                     # initialize the Cube object
cube.load("en")                 # select the desired language (it will auto-download the model)
text="This is the text I want segmented, tokenized, lemmatized, tagged and parsed. It can contain any number of sentences."
sentences=cube(text)            # call with your own text (string)
```
The ``sentences`` object now contains the annotated text; it is a list of sentences, where each sentence is a list of tokens. Each token is a ``ConllEntry`` object that contains individual annotations. The annotation follow the Conll format. 

```python
for sentence in sentences:      # each sentence is a list of tokens
    print()                     # new sentence starts here
    for token in sentence:      # each token is a list of ConllEntry objects        
        print(token.index)       # this is the word position in the sentence
        print(token.word)        # the word itself
        print(token.lemma)       # the word's lemma
        print(token.upos)        # the Universal Part of Speech
        print(token.xpos)        # the language specific Part of Speech 
        print(token.attrs)       # extended set of attributes
        print(token.head)        # the word's head as an index (forms a parse tree)
        print(token.label)       # the label attached to the link between this word and its head 
        print(token.deps)        # dependents
        print(token.space_after) # whether or not has a space after (this is a string in the Conll format)  
```


### Webserver Usage 

To use NLP-Cube as a **web service**, you need to 
[locally install NLP-Cube](examples/2.%20Advanced%20usage%20-%20NLP-Cube%20local%20installation.ipynb) 
and start the server:

For example, the following command will start the server and preload languages: en, fr and de.
```bash
cd cube
python3 webserver.py --port 8080 --lang=en --lang=fr --lang=de
``` 

To test, open the following [link](http://localhost:8080/nlp?lang=en&text=This%20is%20a%20simple%20test) (please copy the address of the link as it is a local address and port link)

## Cite

If you use NLP-Cube in your research we would be grateful if you would cite the following paper: 
* [**NLP-Cube: End-to-End Raw Text Processing With Neural Networks**](http://www.aclweb.org/anthology/K18-2017), Boroș, Tiberiu and Dumitrescu, Stefan Daniel and Burtica, Ruxandra, Proceedings of the CoNLL 2018 Shared Task: Multilingual Parsing from Raw Text to Universal Dependencies, Association for Computational Linguistics. p. 171--179. October 2018 

or, in bibtex format: 

```bib
@InProceedings{boro-dumitrescu-burtica:2018:K18-2,
  author    = {Boroș, Tiberiu  and  Dumitrescu, Stefan Daniel  and  Burtica, Ruxandra},
  title     = {{NLP}-Cube: End-to-End Raw Text Processing With Neural Networks},
  booktitle = {Proceedings of the {CoNLL} 2018 Shared Task: Multilingual Parsing from Raw Text to Universal Dependencies},
  month     = {October},
  year      = {2018},
  address   = {Brussels, Belgium},
  publisher = {Association for Computational Linguistics},
  pages     = {171--179},
  abstract  = {We introduce NLP-Cube: an end-to-end Natural Language Processing framework, evaluated in CoNLL's "Multilingual Parsing from Raw Text to Universal Dependencies 2018" Shared Task. It performs sentence splitting, tokenization, compound word expansion, lemmatization, tagging and parsing. Based entirely on recurrent neural networks, written in Python, this ready-to-use open source system is freely available on GitHub. For each task we describe and discuss its specific network architecture, closing with an overview on the results obtained in the competition.},
  url       = {http://www.aclweb.org/anthology/K18-2017}
}
```
