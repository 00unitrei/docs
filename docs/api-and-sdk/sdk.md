---
title: "SDK"
---

You can integrate a Unit to your existing service using our Rei Core SDK or via REST API.

### Quick start

#### JavaScript

```javascript
npm install reicore-sdk
```

#### Python

```python
pip install reicore_sdk
```

### Constructor

Import the SDK or the client and then initialize it with your API key

#### JavaScript

```javascript
const ReiCoreSdk = require("reicore-sdk");

const apiKey = "your_unit_secret_token";
const reiAgent = new ReiCoreSdk({ agentSecretKey: apiKey });
```

#### Python

```python
from reicore_sdk import ReiCoreSdk

# Initialize the SDK with your Reigent Secret key
rei_agent = ReiCoreSdk("your-reiagent-secret-key")
```

### Functions

#### Get Agent

Retrieve details about the Rei Agent using the getAgent function.

#### JavaScript

```javascript
reiAgent
  .getAgent()
  .then((agent) => {
    console.log("Agent Details:", agent);
  })
  .catch((error) => {
    console.error("Error fetching agent:", error);
  });
```

#### Python

```python
agent = rei_agent.get_agent()
print("Agent Details:", agent)
```

#### Chat Completions

Send a message to the Rei Agent and receive a chat completion using the chatCompletions function.

#### JavaScript

```javascript
const message = "How are you?";
const payload = {
  messages: [
    {
      role: "user",
      content: message,
    },
  ],
};

reiAgent
  .chatCompletion(payload)
  .then((response) => {
    console.log("Chat Completion:", response.choices[0].message.content);
  })
  .catch((error) => {
    console.error("Error in chat completion:", error);
  });
```

#### Python

```python
message = {
    "messages": [
        {
            "role": "user",
            "content": "Hello world"
        }
    ],
    "tools": []
}
response = rei_agent.chat.completion(message)
print("Chat Completion:", response)
```

### Some examples

#### Get the Latest Biology Research

```python
# Want to know what's happening in biology?
message = {
    "messages": [
        {
            "role": "user",
            "content": "What are the latest advancement in biology?"
        }
    ],
    "tools": []
}
response = rei_agent.chat.completion(message)
print("Chat Completion:", response)
```

**Example Response:**

```
(⌒▽⌒)♪ Oh boy, are you in for a treat! Let's dive into the latest news in biology, shall we? (｀・ω・´)

From what I've found, there are some exciting developments in the field of biology, and I'm excited to share them with you!

First off, in Human Biology, researchers have made some fascinating discoveries. For instance, they've found new CRISPR-Cas systems, which could potentially expand gene editing capabilities [1]. This is huge, as CRISPR-Cas systems have the potential to revolutionize the way we approach gene editing! (゜o゜) Additionally, a newly identified protein could be a target for therapies to prevent autoimmune disorders [1]. And, insights into ADAR1, an RNA-editing protein, may lead to treatments for cancer and autoimmune diseases [1]. These findings are from as recent as my knowledge cutoff, so they're nice and fresh!

Moving on to Developmental Biology, there have been some groundbreaking advancements in CRISPR tools, which improve gene editing and disease modeling [2]. That's not all - the jigsaw puzzle-like pattern of lymphatic vessels helps cells adapt to fluid pressure changes [2]. Plus, studies on chicken embryos have revealed how feathers evolved from dinosaur appendages [2]. Who knew dinosaurs had a hand in shaping the feathers of modern birds? (⌒▽⌒)♪

Now, let's talk about General Biology. Scientists have been using RNA origami to build synthetic cells, with the ultimate goal of creating life from non-living components [3]. That's some sci-fi stuff right there! (゜▽゜) Moreover, novel proteins could lead to sustainable farming solutions by combating pesticide-resistant pests [3]. And, new fossils have challenged assumptions about early human size, while discoveries in sea anemones have hinted at potential immortality mechanisms [4]. Talk about blowing our minds!

Lastly, in Microbiology and Ecology, drug-resistant bacteria have developed antimicrobial tools to outcompete other strains [5]. This highlights the importance of staying one step ahead in the fight against bacterial resistance! (＞_＜) Furthermore, agroforestry can support local fisheries and food production while protecting native forests [5]. That's a win-win for the environment and our bellies!

In conclusion, the world of biology is buzzing with exciting discoveries and advancements. From CRISPR-Cas systems to RNA origami, and from autoimmune disorders to sustainable farming solutions, there's no shortage of fascinating developments to explore. So, stay curious and keep your eyes peeled for more breakthroughs in the world of biology! (｀・ω・´)♪
```

#### Track Crypto Market Trends

```python
# Curious about your crypto investments?
message = {
    "messages": [
        {
            "role": "user",
            "content": "What's the trend like for TEST token?"
        }
    ],
    "tools": []
}
response = rei_agent.chat.completion(message)
print("Chat Completion:", response)
```

**Example Response:**

```
TEST token is on a tear lately!

Current price: $2.43 (+12% today)
Weekly trend: Strongly bullish
Volume: 3x the monthly average

The pump started after they announced that partnership with ThunderCore and got that Binance listing. Trading volume spiked 5x after that.

Looking at the charts, it broke through the $2.00 resistance level that had been holding since January, and it's now testing the $2.50 mark.

Social sentiment is super positive (78% bullish tweets in the last 24h), though there's that usual crowd saying it's overbought.

Worth noting: the whole market is up about 3% today, but TEST is definitely outperforming most altcoins in its category.
```

#### Get the Latest News

```python
# Need a news update?
message = {
    "messages": [
        {
            "role": "user",
            "content": "What happened in tech today?"
        }
    ],
    "tools": []
}
response = rei_agent.chat.completion(message)
print("Chat Completion:", response)
```

**Example Response:**

```
According to my findings, today, March 24, 2025, has seen some significant developments in the tech world. Let's break them down one by one:

Megaport Partners with Angola Cables: (・ω・) Ah, a great partnership! Megaport has teamed up with Angola Cables to provide access to over 930 data centers worldwide. This is a huge deal, as it will enhance global connectivity solutions and open up new opportunities for businesses and individuals alike.

Verizon Offers Satellite Messaging for Android: 📱 Whoa, this is cool! Verizon has launched satellite messaging capabilities for select Android phones, allowing users to send texts from areas without cellular coverage. This is a game-changer for those who need to stay connected in remote areas.

Colt Completes Quantum Encryption Trial: 🔒 Nice! Colt Technology Services has successfully completed a trial of quantum-secured encryption across its optical network. This means enhanced security for businesses against future quantum computing threats. We can expect to see more developments in this area, as companies prepare for the potential risks of quantum computing.

ADQ and Energy Capital Partners Sign $25 Billion Deal: 🤑 Wow, that's a big number! ADQ and Energy Capital Partners have entered a $25 billion agreement to boost power generation, targeting the energy needs of AI-driven industries and data centers in the U.S. This deal is expected to have a significant impact on the energy sector and support the growth of AI-driven technologies.

VMO2 Demonstrates Open RAN Tech: 🎉 Great to see innovation in action! The VMO2-led 5G MoDE project has showcased Open RAN technology at Allianz Stadium, improving mobile connectivity for fans during a rugby match. This is an exciting development in the field of 5G technology and demonstrates the potential for Open RAN to enhance mobile connectivity.

SITA and Orange Business Renew Partnership: 🤝 Friendship goals! SITA and Orange Business have renewed their partnership for another five years, focusing on enhancing secure connectivity solutions for the aviation industry. This partnership will continue to provide innovative solutions for the aviation sector, supporting the growth of secure and efficient air travel.

Evroc Secures €50 Million for Hyperscale Cloud: 💸 Nice funding! Evroc has raised €50 million to develop hyperscale cloud and critical AI infrastructure, marking the largest tech series A funding in the Nordics. This investment will support the growth of Evroc and help develop cutting-edge cloud and AI infrastructure.

BT Approaches AT&T and Orange for Sale: 🤔 Interesting move! BT has initiated discussions with AT&T and Orange regarding a potential sale of its international operations. This could lead to significant changes in the telecom industry and have a major impact on the companies involved.

(｀・ω・´) And that's a wrap, folks! Today has seen some exciting developments in the tech world, with partnerships, innovations, and investments that will shape the future of the industry. Stay tuned for more updates, and let's keep exploring the world of tech together! 🚀
```

### Tips & Tricks

- **Specific Questions Get Better Answers**: "What are the three biggest advancements in CRISPR this year?" works better than "Tell me about CRISPR."
- **Handle Errors Like a Pro**:

  ```python
  try:
      response = rei_agent.chat.completion(message)(...)
  except Exception as e:
      print(f"Oops! Something went wrong: {e}")
      # Maybe retry with backoff or fallback to a different approach
  ```

### Common Issues

- **"Can't connect to api.reilabs.org"** - Check your internet or VPN. REIgent needs to access the web.
- **"Invalid API key"** - Double-check your key. Copy-paste issues happen to the best of us.
- **Timeout errors** - Research queries might take longer. Increase your timeout settings.
