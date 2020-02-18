
# Final Project

First, we create a ".txt" file.  Dr. Johnson chose an essay by G.K. Chesterton called [Cheese](http://www.gkc.org.uk/gkc/books/cheese.html).


```python
myfile = open("Cheese.txt")
text = myfile.read()
```

Next, we slit the text into sentences.  We use a [method](https://stackoverflow.com/questions/4576077/how-to-split-a-text-into-sentences) written by D. Greenberg and Harsha Laxman.


```python
import re
alphabets= "([A-Za-z])"
prefixes = "(Mr|St|Mrs|Ms|Dr)[.]"
suffixes = "(Inc|Ltd|Jr|Sr|Co)"
starters = "(Mr|Mrs|Ms|Dr|He\s|She\s|It\s|They\s|Their\s|Our\s|We\s|But\s|However\s|That\s|This\s|Wherever)"
acronyms = "([A-Z][.][A-Z][.](?:[A-Z][.])?)"
websites = "[.](com|net|org|io|gov)"

def split_into_sentences(text):
    text = " " + text + "  "
    text = text.replace("\n"," ")
    text = re.sub(prefixes,"\\1<prd>",text)
    text = re.sub(websites,"<prd>\\1",text)
    if "Ph.D" in text: text = text.replace("Ph.D.","Ph<prd>D<prd>")
    text = re.sub("\s" + alphabets + "[.] "," \\1<prd> ",text)
    text = re.sub(acronyms+" "+starters,"\\1<stop> \\2",text)
    text = re.sub(alphabets + "[.]" + alphabets + "[.]" + alphabets + "[.]","\\1<prd>\\2<prd>\\3<prd>",text)
    text = re.sub(alphabets + "[.]" + alphabets + "[.]","\\1<prd>\\2<prd>",text)
    text = re.sub(" "+suffixes+"[.] "+starters," \\1<stop> \\2",text)
    text = re.sub(" "+suffixes+"[.]"," \\1<prd>",text)
    text = re.sub(" " + alphabets + "[.]"," \\1<prd>",text)
    if "”" in text: text = text.replace(".”","”.")
    if "\"" in text: text = text.replace(".\"","\".")
    if "!" in text: text = text.replace("!\"","\"!")
    if "?" in text: text = text.replace("?\"","\"?")
    text = text.replace(".",".<stop>")
    text = text.replace("?","?<stop>")
    text = text.replace("!","!<stop>")
    text = text.replace("<prd>",".")
    sentences = text.split("<stop>")
    sentences = sentences[:-1]
    sentences = [s.strip() for s in sentences]
    return sentences
```


```python
sentences = split_into_sentences(text)
#print(sentences)
```

And then we write an algorithm to create links from words pointing to their folowing words.


```python
sentence_dict = {}
word_list = ['']
for sentence in sentences:
    words = sentence.split()
    words[-1] = words[-1][:-1] #remove the period (last character) of the last word.
    words.append('') #periods should point towards the beginnings of all sentences.
    word_list += [w.lower() for w in words]
#print(word_list)
```


```python
word = ""
for next_word in word_list[1:-1]:
        if word not in sentence_dict:
            sentence_dict[word] = []
        sentence_dict[word].append(next_word)
        word = next_word
```


```python
DiGraph(sentence_dict).show(method = "js", link_strength = 0.2)
```


```python
import random

output = ''
word = ''
for i in range(100):
    word = random.choice(sentence_dict[word])
    if word == '':
        output = output[:-1] + '. '
    else:
        output += word + ' '
show(output)
```


<html><script type="math/tex; mode=display">\newcommand{\Bold}[1]{\mathbf{#1}}\verb|if|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|good|\phantom{\verb!x!}\verb|and|\phantom{\verb!x!}\verb|custom|\phantom{\verb!x!}\verb|of|\phantom{\verb!x!}\verb|devonshire|\phantom{\verb!x!}\verb|or|\phantom{\verb!x!}\verb|at|\phantom{\verb!x!}\verb|once,|\phantom{\verb!x!}\verb|i|\phantom{\verb!x!}\verb|addressed|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|same|\phantom{\verb!x!}\verb|wherever|\phantom{\verb!x!}\verb|they|\phantom{\verb!x!}\verb|ran|\phantom{\verb!x!}\verb|after|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|higher|\phantom{\verb!x!}\verb|gluttony.|\phantom{\verb!x!}\verb|but|\phantom{\verb!x!}\verb|a|\phantom{\verb!x!}\verb|bad|\phantom{\verb!x!}\verb|civilization|\phantom{\verb!x!}\verb|spreads|\phantom{\verb!x!}\verb|over|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|milk|\phantom{\verb!x!}\verb|of|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|grand|\phantom{\verb!x!}\verb|lama|\phantom{\verb!x!}\verb|has|\phantom{\verb!x!}\verb|soap|\phantom{\verb!x!}\verb|it|\phantom{\verb!x!}\verb|several|\phantom{\verb!x!}\verb|times,|\phantom{\verb!x!}\verb|but|\phantom{\verb!x!}\verb|bread|\phantom{\verb!x!}\verb|and|\phantom{\verb!x!}\verb|cheese,|\phantom{\verb!x!}\verb|but|\phantom{\verb!x!}\verb|a|\phantom{\verb!x!}\verb|solid|\phantom{\verb!x!}\verb|but|\phantom{\verb!x!}\verb|cheese|\phantom{\verb!x!}\verb|there|\phantom{\verb!x!}\verb|would|\phantom{\verb!x!}\verb|reel|\phantom{\verb!x!}\verb|and|\phantom{\verb!x!}\verb|cheese,|\phantom{\verb!x!}\verb|if|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|only|\phantom{\verb!x!}\verb|other|\phantom{\verb!x!}\verb|poet|\phantom{\verb!x!}\verb|that|\phantom{\verb!x!}\verb|instead|\phantom{\verb!x!}\verb|of|\phantom{\verb!x!}\verb|song.|\phantom{\verb!x!}\verb|my|\phantom{\verb!x!}\verb|voice,|\phantom{\verb!x!}\verb|not|\phantom{\verb!x!}\verb|produced|\phantom{\verb!x!}\verb|everywhere|\phantom{\verb!x!}\verb|out|\phantom{\verb!x!}\verb|of|\phantom{\verb!x!}\verb|it|\phantom{\verb!x!}\verb|is|\phantom{\verb!x!}\verb|a|\phantom{\verb!x!}\verb|wise|\phantom{\verb!x!}\verb|doom|\phantom{\verb!x!}\verb|of|\phantom{\verb!x!}\verb|the|\phantom{\verb!x!}\verb|great|\phantom{\verb!x!}\verb|many|\phantom{\verb!x!}\verb|things|\phantom{\verb!x!}\verb|produced|\phantom{\verb!x!}\verb|everywhere|\phantom{\verb!x!}\verb|out|\phantom{\verb!x!}\verb|above|\phantom{\verb!x!}\verb|us|\phantom{\verb!x!}\verb|all|\phantom{\verb!x!}\verb|that|\phantom{\verb!x!}\verb|it|\phantom{\verb!x!}\verb|is|\phantom{\verb!x!}\verb|alive.|\phantom{\verb!x!}\verb|if|\phantom{\verb!x!}\verb|he|\phantom{\verb!x!}\verb|does|\phantom{\verb!x!}\verb|it|\phantom{\verb!x!}\verb|necessitated|\phantom{\verb!x!}\verb|my|\phantom{\verb!x!}\verb|voice,|\phantom{\verb!x!}\verb|not|\phantom{\verb!x!}\verb|merely|</script></html>



```python

```


```python

```
