## Overview

The Cloud Natural Language API lets you extract entities from text, perform sentiment and syntactic analysis, and classify text into categories.

### Create an API key

1. Navigate to **APIs & Services** > **Credentials**
2. Click **Create Credentials** and select **API key**
3. Copy the generated API key
4. SSH into your instance
5. Save your API_KEY into variable in the shell

#### Make an entity analysis request

analyzeEntities API can extract entities (like people, places, and events) from text.

Construct a request payload and save in request.json

```json
{
  "document": {
    "type": "PLAIN_TEXT",
    "content": "Joanne Rowling, who writes under the pen names J. K. Rowling and Robert Galbraith, is a British novelist and screenwriter who wrote the Harry Potter fantasy series."
  },
  "encodingType": "UTF8"
}
```

#### Call the Natural Language API Entity Analysis

Pass request body, along with the API key environment variable to the Natural Language API with the following curl command

```shell
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json > result.json

cat result.json
```

#### Make a sentiment analysis request

In addition to extracting entities, the Natural Language API also lets you perform sentiment analysis on a block of text.

Construct a request payload and save in request.json

```json
{
  "document": {
    "type": "PLAIN_TEXT",
    "content": "Harry Potter is the best book. I think everyone should read it."
  },
  "encodingType": "UTF8"
}
```

#### Call the Natural Language API Sentiment Analysis

Pass request body, along with the API key environment variable to the Natural Language API with the following curl command

```shell
curl "https://language.googleapis.com/v1/documents:analyzeSentiment?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

#### Analyzing entity sentiment

In addition to providing sentiment details on the entire text document, the Natural Language API can also break down sentiment by the entities in the text

Construct a request payload and save in request.json

```json
{
  "document": {
    "type": "PLAIN_TEXT",
    "content": "I liked the sushi but the service was terrible."
  },
  "encodingType": "UTF8"
}
```

#### Call the Natural Language API Analyzing entity sentiment

Pass request body, along with the API key environment variable to the Natural Language API with the following curl command

```shell
curl "https://language.googleapis.com/v1/documents:analyzeEntitySentiment?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

#### Analyzing syntax and parts of speech

syntactic analysis, another of the Natural Language API's methods, to dive deeper into the linguistic details of the text. analyzeSyntax extracts linguistic information, breaking up the given text into a series of sentences and tokens (generally, word boundaries), to provide further analysis on those tokens. For each word in the text, the API tells you the word's part of speech (noun, verb, adjective, etc.) and how it relates to other words in the sentence (Is it the root verb? A modifier?).

Construct a request payload and save in request.json

```json
{
  "document": {
    "type": "PLAIN_TEXT",
    "content": "Joanne Rowling is a British novelist, screenwriter and film producer."
  },
  "encodingType": "UTF8"
}
```

#### Call the Natural Language Analyzing syntax and parts of speech

Pass request body, along with the API key environment variable to the Natural Language API with the following curl command

```shell
curl "https://language.googleapis.com/v1/documents:analyzeSyntax?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```
