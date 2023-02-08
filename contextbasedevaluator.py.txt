import re, math
import numpy as np
from tkinter import messagebox
import PIL.Image
import pytesseract
from collections import Counter
from tkinter import *
from tkinter.filedialog import askopenfilename
from tkinter import simpledialog
import tkinter
from tkinter import messagebox
root = tkinter.Tk()
root.title(&quot;Subjective Answer Evaluation Using Machine Learning&quot;)
root.geometry(&quot;800x500&quot;)

global answer_path
global image_path
global answer
global student_answer

from collections import defaultdict
vector = defaultdict(list)
arrayList = []
WORD = re.compile(r&#39;\w+&#39;)

def text_to_vector(text):
words = WORD.findall(text)
return Counter(words)

def (words):
w=WordNetLemmatizer()

words=list(map(w.lemmatize,words))
return words

def get_cosine(vec1, vec2):
intersection = set(vec1.keys()) &amp; set(vec2.keys())
numerator = sum([vec1[x] * vec2[x] for x in intersection])

sum1 = sum([vec1[x]**2 for x in vec1.keys()])
sum2 = sum([vec2[x]**2 for x in vec2.keys()])
denominator = math.sqrt(sum1) * math.sqrt(sum2)

if not denominator:
return 0.0
else:
return float(numerator) / denominator

def uploadAnswers():
global answer_path
global answer
answer = &quot;&quot;
answer_path = askopenfilename(initialdir = &quot;Answers&quot;)
answerlabel.config(text=answer_path)
with open(answer_path, &quot;r&quot;) as file:
for line in file:
line = line.strip(&#39;\n&#39;)
line = line+&quot; &quot;
answer+=line

def uploadImage():
global image_path
global student_answer

image_path = askopenfilename(initialdir = &quot;images&quot;)
student_answer = pytesseract.image_to_string(PIL.Image.open(image_path))
text.delete(&#39;1.0&#39;,END);
text.insert(END,student_answer+&quot;\n\n&quot;)
print(student_answer)

def evaluate():
grade =&quot;&quot;
vector = text_to_vector(answer)
vector1 = lematize(vector1)
vector2 = text_to_vector(student_answer)
cosine = get_cosine(vector1, vector2)
cosine = cosine * 100;
if cosine &gt;= 90:
grade = &quot;Excellent&quot;
if cosine &gt;= 80 and cosine &lt; 90:
grade = &quot;Very Good&quot;
if cosine &gt;= 70 and cosine &lt; 80:
grade = &quot;Good&quot;
if cosine &gt;= 50 and cosine &lt; 70:
grade = &quot;Ok&quot;
if cosine &gt;= 35 and cosine &lt; 50:
grade = &quot;Poor&quot;
if cosine &lt; 35:
grade = &quot;Very Poor&quot;
req = requests.get(&quot;https://api.textgears.com/check.php?text=&quot; + answer +
&quot;&amp;key=JmcxHCCPZ7jfXLF6&quot;)
no_of_errors = len(req.json()[&#39;errors&#39;])
if no_of_errors &gt; 5:
g = 0
else:

g = 1
q = math.ceil(fuzz.token_set_ratio(answers, student_answer) * 6 / 100)
predicted = nav_test.predict(k, g, q)/
result = predicted * out_of / 10
text.insert(END,&quot;Your score : &quot;+str(result)+&quot;\n&quot;)
text.insert(END,&quot;Your Grade : &quot;+str(grade)+&quot;\n&quot;)
messagebox.showinfo(&quot;Dataset filtered successfully&quot;,&quot;Your score : &quot;+str(result)+&quot;\nYour Grade :
&quot;+grade)

upload = Button(root, text=&quot;Upload Prebuilt Answers&quot;, command=uploadAnswers)
upload.grid(row=0)

answerlabel = Label(root)
answerlabel.grid(row=1)

imagebutton = Button(root, text=&quot;Upload Student Answer Image&quot;, command=uploadImage)
imagebutton.grid(row=2)

evaluate = Button(root, text=&quot;Evaluate Answer&quot;, command=evaluate)
evaluate.grid(row=3)

text=Text(root,height=30,width=120)
scroll=Scrollbar(text)
text.configure(yscrollcommand=scroll.set)
text.grid(row=5)

root.mainloop()