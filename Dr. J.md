
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
#uncomment the line below to see a cool directed graph of all the words.
#DiGraph(sentence_dict).show(method = "js", link_strength = 0.2)
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
print(output)
```

    the four wayside public-houses i hope (though i was so supercilious as in the civilization of cheese was only just here that a cheshire cheese in an exalted poetry. once in endeavouring to his prayers, he brought me as in the fall. there was only other poet that is nothing subtly and cheese' - which is brown's soap it is brown's soap it is with a local cheese, indeed, but yielding because it rhymes to raise my voice, not lightly to `breeze' and `seas' (an essential point); that the waiter in endeavouring to understand that are not worthy), 



```python

```


```python

```
