# question
#!/usr/bin/python

import os
import pickle
import re
import sys
from parse_out_email_text import parseOutText

"""
    Starter code to process the emails from Sara and Chris to extract
    the features and get the documents ready for classification.

    The list of all the emails from Sara are in the from_sara list
    likewise for emails from Chris (from_chris)

    The actual documents are in the Enron email dataset, which
    you downloaded/unpacked in Part 0 of the first mini-project. If you have
    not obtained the Enron email corpus, run startup.py in the tools folder.

    The data is stored in lists and packed away in pickle files at the end.
"""


from_sara  = open("from_sara.txt", "r")
from_chris = open("from_chris.txt", "r")

from_data = []
word_data = []

### temp_counter is a way to speed up the development--there are
### thousands of emails from Sara and Chris, so running over all of them
### can take a long time
### temp_counter helps you only look at the first 200 emails in the list so you
### can iterate your modifications quicker
temp_counter = 0


for name, from_person in [("sara", from_sara), ("chris", from_chris)]:
    for path in from_person:
        ### only look at first 200 emails when developing
        ### once everything is working, remove this line to run over full dataset
        temp_counter += 1
        if temp_counter < 200:
            path = os.path.join('enron_mail_20150507/', path[:-1])
            #print path
            email = open(path, "r")

            ### use parseOutText to extract the text from the opened email

            ### use str.replace() to remove any instances of the words
            ### ["sara", "shackleton", "chris", "germani"]

            ### append the text to word_data

            ### append a 0 to from_data if email is from Sara, and 1 if email is from Chris

            text = parseOutText(email)
            words = text
            words_list = words.strip().split()
            
            #print words_list
            #===============================================  How can I make the number of word_data equal the number of from data =====================
            for i in words_list:
                if i == 'Sara':
                    from_data.append(0)
                    words_list[words_list.index(i)]=''
                elif i == 'Chris':
                    from_data.append(1)
                    words_list[words_list.index(i)]=''
                elif i == 'shackleton':
                    words_list[words_list.index(i)]=''
                elif i == 'germani':
                    words_list[words_list.index(i)]=''
            #============================================================================================================================================        

            
            words_list_1 = []
            #print words_list,'\n'
            for i in words_list:
                if i=='':
                    continue
                else:
                    words_list_1.append(i)
                    
            words_new_list = []
            from nltk.stem.snowball import SnowballStemmer
            stemmer = SnowballStemmer('english')
            for i in words_list_1:
                words_new_list.append(stemmer.stem(i))
                
            new_words = ' '.join(words_new_list)
            word_data.append(new_words)

            
            email.close()

#print "emails processed"
from_sara.close()
from_chris.close()

#pickle.dump( word_data, open("your_word_data.pkl", "w") )
#pickle.dump( from_data, open("your_email_authors.pkl", "w") )





### in Part 4, do TfIdf vectorization here


print 'len(from_data):',len(from_data)
print 'len(word_data)',len(word_data)
