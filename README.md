# Media Semantics ChatGPT Agent Reference Implementation
Embodied Agent using ChatGPT, Character API, and AWS Polly.

## Overview
This project uses OpenAI's completions API, along with the [Character API](https://aws.amazon.com/marketplace/pp/B06ZY1VBFZ), a cloud-based Character Animation API available on the Amazon AWS Marketplace, in order to build a chatbot with a body and voice.

You can see the Reference Implementation running [here](https://mediasemantics.com/chatgptagent.html). 

Other places to start include the [Character API](https://github.com/mediasemantics/charapi) and the [Real-Time Agent](https://github.com/mediasemantics/realtimeagent) Reference Implementations.


## Requirements
This readme describes how to install the project on your local machine. It assumes bash syntax, but can be installed on Linux, Mac, or 
Windows. You must have NodeJS 22 or later installed. You will also need an AWS account with suitable permissions.

## Obtaining keys
You will need an [Open AI](https://openai.com/) account with a key for the "chat/completions" endpoint. You will be charged based on usage.

Use this [AWS Markeplace](https://aws.amazon.com/marketplace/pp/B06ZY1VBFZ) page to add the
Character API service to your AWS account. You will receive credentials by email to log onto your API dashboard, where you will generate an API key. You will be charged $0.007 per call to the Character API. There are no monthly minimums. 
Charges will appear on your monthly AWS bill. 

This sample uses the Amazon Polly Text-to-Speech API, which is also priced based on usage. 
To access the AWS Polly Text-to-Speech service, you will want to create an IAM User for your app. On the AWS Console, go to the IAM service, click Users and then "Create User". Provide a name, such as "github_sample".
Press Next. Select "Attach policies directly". Then, in the Permissions policies Search field, type "polly". Select the checkbox next to AmazonPollyFullAccess. Click Next. Optionally create a tag, then click Create User.
Next, click on the newly created user to open it, and click the "Security credentials" tab. Scroll down to the section labeled "Access keys" and press "Create access key". Click Other, then Next. Then press "Create access key".
You will want to copy two strings. The Access key ID is a string of capitalized alphanumeric characters, and the Secret Access Key is longer string of mixed-case characters. Make sure you record both values, as you will need to insert them in the steps below.

## Installing the Server
Install the sample in a directory on the server, e.g. the home directory:
```
$ cd ~  
$ git clone https://github.com/mediasemantics/chatgptagent.git  
$ cd chatgptagent
```

Install the required dependencies:
```
$ npm install
```

Set required keys:
```
$ export OPENAI_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
$ export CHARACTER_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxx"
$ export POLLY_ACCESS_KEY_ID="xxxxxxxxxxxxxxxxxxxx",
$ export POLLY_SECRET_ACCESS_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
$ export POLLY_REGION="us-east-1",
```

Start the server with:
```
$ node server.js
```
You should see "Listening on port 3000".

## Installing the Client

Open a second command window and insure the client module dependencies are installed.
```
$ cd ~/chatgptagent/html
$ npm install
```

Install and run a local file server.
```
$ npm install -g http-server
$ cd ~/chatgptagent/html
$ http-server . -p 3001
```

Point your browser to: http://localhost:3001/chatgptagent.html

## How it works

This sample builds on the [Character API Reference Implementation](https://github.com/mediasemantics/charapi) github project. Below the character, a simple input field and submit button lets the user enter text. This text is sent to the 'chat' endpoint in server.js, along with a userid that is generated when the page first loads. The server takes the input and sends it to the ChatGPT completions api to obtain a response. When the response text returns to the client, it is split into sentences. Each sentence is passed to the dynamicPlay API, as described in the Character API Reference Implementation. The user's chat history is stored in a text file so that it can be included in subsequent ChatGPT requests, to allow for continuity in the conversation.
