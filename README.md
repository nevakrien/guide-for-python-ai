# guide-for-python-ai

this guide is trying to make it possible to get a stable relible code base using python.
there is a lot of hidden knowledge that goes into actually deploying a long term working code base.
I have personally ran into these issues again and again interning at intel

from exprince working on AI reaserch can on a bad day be 90% just trying to make code work again so  you can reproduce an expirement.

I have NEVER had a python AI project that worked the same 6 months later without manual intervention.
naturally when building a relible service this is an absolute horror.

now this sounds weird because python SHOULD be cross platform. but...

## Python isn't cross platform

idk how many times I would need to scream this but PYTHON ENVIRONMENTS BREAK, usually for no reason. idc if its the same computer tomorrow running
```bash
    pip install -r requirements.txt
```
can always break for seemingly no reason or completely ruin your current environment.

if your current environment is important to you DO NOT INSTALL IN IT because you are risking it breaking.
so whenver you wana add something to an existing important enviorment ALLWAYS clone it first then work with the clone. idc if it takes a full hour its allways worth it. the risk for not doing so is just to high.

this super annoying issue is caused by how pypi works. when you pip install python packages you are asking a constantly changing server for information and then running arbitrary code and hoping for the best. miraculously this usually works

this naturally leads me to the second rule

## Always save the original
as disscused python breaks. so if you need to have python code work NOW the only real way to do so relibely is have the excact same computer. having a machine which has all your packages already installed and wokring is super useful.

at my internship in intel we ended up cloning 50 diffrent computers (which took 3 hours) because  porting the code took us a combined 24+ hours of work we simply did not have. worse you cant tell its gona work in 24 hours you kinda just have to prey. while cloning a hard drive is relibly gona work every time.

if you cant save the hard drive saving a virtual enviorment is the next best thing. I would actually recommend commiting it to git so its easy to track changes and revert if needed. since again without saving the actual envioremnt there is no way to get it again.

## Libararies
there are a lot of useful AI libraries some of them break more often then others.
hugginface is by far the most usefull and the least relible libarary.
this naturally causes a lot of tention and headaches.

getting models out of huggingface and into something on disk like openvino onnex or GUFF is a huge upgrade in relibiliy. runing one of these more stable libararies significantly lowers the risk of random breakage over time.

in general the less depencies the better. this is espcially true for second order depncies which you naturally have less control over.

the libararies I like to use instead of hugginface are
1. openvino_genai
2. openvino
3. keras_nlp 
4. ollama or other HTTP apis 
5. huggingface

now usually you would end up needing:
1. numpy 
2. pytorch
3. hugginface

there is a specific order in which you should install these. you need to first make sure you have a fresh empty venv of the correct python version that supports pytorch. usually being 1 or 2 versions behind is a good idea so if pytorch can do 3.12 you wana be on 3.11 or 3.10

you then install pytorch for your specific system, intel used to require ipex for it to work on GPU but thats no longer needed. AMD needs to be installed from its own branch as well. 

then after you have pytorch you install hugginface and other dependent packages.
its usually a good idea to keep track of the order in which you installed packages because again this matters.

so instead of directly calling pip install puting things at the bottom of the requirment file is the better way to go. then "pip install -r requirments.txt"


# Fixing breakage
when something invetibly breaks with versions this is the way I usually fix it.

1. try and play with transformers version to fit 
2. try and fix numpy version
3. start from scratch with new order of things
4. try random changes for a while
5. consider giving up and using something else.

it helps a lot if you have friends/co-workers that work in the space because they sometimes just know the bugs you run into. note that these bugs change every month so remebering the details isnt that impotatant in the longrun 