# keras-chatbot-web-api

Simple keras chat bot using seq2seq model with Flask serving web

The chat bot is built based on seq2seq models, and can infer based on either character-level or word-level. 

The seq2seq model is implemented using LSTM encoder-decoder on Keras. 

# Usage

Run the following command to install the keras, flask and other dependency modules:

```bash
sudo pip install -r requirements.txt
```

The chat bot models are chained using cornell-dialogs and gunthercox-corpus data set and are available in the 
chat bot_train/models directory. During runtime, the flask app will load these trained models to perform the 
chat-reply

Goto chatbot_web directory and run the following command:

```bash
python flaskr.py
```

Now navigate your browser to http://localhost:5000 and you can try out various predictors built with the following
trained seq2seq models:

* Character-level seq2seq models
* Word-level seq2seq models

To make the bot reply using web api, after the flask server is started, run the following curl POST query
in your terminal:

```bash
curl -H 'Content-Type application/json' -X POST -d '{"level":"level_type", "sentence":"your_sentence_here", "dialogs":"chatbox_dataset"}' http://localhost:5000/chatbot_reply
```

The level_type can be "char" or "word", the dialogs can be "gunthercox" or "cornell"

(Note that same results can be obtained by running a curl GET query to http://localhost:5000/chatbot_reply?sentence=your_sentence_here&level=level_type&dialogs=chatbox_dataset)

For example, you can ask the bot to reply the sentence "How are you?" by running the following command:

```bash
curl -H 'Content-Type: application/json' -X POST -d '{"level":"word", "sentence":"How are you?", "dialogs":"gunthercox"}' http://localhost:5000/chatbot_reply
```

And the following will be the json response:

```json
{
    "dialogs": "gunthercox",
    "level": "word",
    "reply": "i am doing well how about you",
    "sentence": "How are you?"
}
```

Here are some examples for eng chat-reply using some other configuration options:

```bash
curl -H 'Content-Type: application/json' -X POST -d '{"level":"char", "sentence":"How are you?", "dialogs":"gunthercox"}' http://localhost:5000/chatbot_reply
curl -H 'Content-Type: application/json' -X POST -d '{"level":"word", "sentence":"How are you?", "dialogs":"cornell"}' http://localhost:5000/chatbot_reply
curl -H 'Content-Type: application/json' -X POST -d '{"level":"char", "sentence":"How are you?", "dialogs":"cornell"}' http://localhost:5000/chatbot_reply
```

# TODO

* Parameter tuning: as the seq2seq usually takes many hours to train on large corpus and long sentences, i don't have sufficient time at the moment to do a good job on the 
parameter tuning and training for a longer period of time (current parameters were tuned such that the bots can be trained in a few hours)
* Better text preprocessing: to improve the bot behavior, one way is to perform more text preprocessing before the training (e.g. stop word filtering, stemming)


