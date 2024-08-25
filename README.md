# Media Semantics ChatGPT Agent
Embodied Agent using ChatGPT, Character API, and AWS Polly.

## Overview
This project uses OpenAI's completions API, along with the [Character API](https://aws.amazon.com/marketplace/pp/B06ZY1VBFZ), a cloud-based Character Animation API available on the Amazon AWS Marketplace, in order to build a chatbot with a body and voice.

You can see the Reference Implementation running [here](https://mediasemantics.com/chatgptagent.html). 

## Requirements
This ReadMe describes how to install the project on your local machine. You must have NodeJS installed.
If you prefer, you can also install the server portion directly on a web server.

## Obtaining keys
You will need an [Open AI](https://openai.com/) account with a key for the "chat/completions" endpoint. You will be charge based on usage.

Use this [AWS Markeplace](https://aws.amazon.com/marketplace/pp/B06ZY1VBFZ) page to add the
Character API service to your AWS account. You will receive credentials by email to log onto your API dashboard. There you will generate an API key that you will insert in the server.js file. You will be charged $0.007 per call to the Character API. There are no monthly minimums. 
Charges will appear on your monthly AWS bill. 

This sample uses the Amazon Polly Text-to-Speech API, which is also priced based on usage. 
Since this sample caches the API results, API fees are only incurred for text that has not already been seen, so your actual spend depends on your traffic and on the effectiveness of the cache.

To access the AWS Polly Text-to-Speech service, you will want to create an IAM User for your app. On the AWS Console, go to the IAM service, click Users and then "Create User". Provide a name, such as "github_sample".
Press Next. Select "Attach policies directly". Then, in the Permissions policies Search field, type "polly". Select the checkbox next to AmazonPollyFullAccess. Click Next. Optionally create a tag, then click Create User.
Next, click on the newly created user to open it, and click the "Security credentials" tab. Scroll down to the section labeled "Access keys" and press "Create access key". Click Other, then Next. Then press "Create access key".
You will want to copy two strings. The Access key ID is a string of capitalized alphanumeric characters, and the Secret Access Key is longer string
of mixed-case characters. Make sure you record both values, as you will need to insert them into the sample.

## Installing the Server
Install the sample in a directory on the server, e.g. the home directory:
```
cd ~  
git clone https://github.com/mediasemantics/chatgptagent.git  
cd chatgptagent
```

Install the required dependencies:
```
npm install
```

Create a cache subdirectory:
```
mkdir cache
```

Next, modify the server.js file to add your Character API, AWS Polly, and OpenAI access credentials.

Replace 'xxxxxxxxxxxxxxxxxxxxxxxxx' with the 25 character API Key from the API Dashboard.

Replace 'xxxxxxxxxxxxxxxxxxxx' and 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' with the values obtained when you created the 'polly' IAM user.

Replace 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' with the key obtained from OpenAI.

Save your changes.

You can now start the server with:
```
node server.js
```
You should see "Listening on port 3000".

## Installing the Client

Open a second command window in the html directory. Ensure the client module is installed.

```
cd ~/chatgptagent/html
npm install
```

Then install and run a local file server.

```
npm install -g http-server
cd ~/chatgptagent/html
http-server . -p 3001
```

Then point your browser to: http://localhost:3001/chatgptagent.html

## How it works

This sample builds on the [Character API Reference Implementation](https://github.com/mediasemantics/charapi) github project. Below the character, a simple input field and submit button let the user enter text. This text is sent to the 'chat' endpoint in server.js, along with a userid that is generated when the page first loads. The server takes the input and sends it to the ChatGPT completions api to obtain a response. The input and the response are then stored at the server. On subsequent rounds, the last few inputs and outputs are included in the ChatGPT request, along with the input, in order to provide for continuity in the conversation.

When the response text returns to the client, it is split into sentences. Each sentence is passed to the dynamicPlay API, as described in the Character API Reference Implementation. The "closedcaption" event is used to populate a chat history, so that each response sentence appears in the history as it is spoken.




