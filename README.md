---
type: concept
domain: narrative
priority: critical
status: in-progress
audience: all
---

# Monitor Data
The main factor in this tech is the data:
We monitor the computer activity, unalike other platforms, we use AI and Eyes to "see" and not only gather intent lacking data.
We monitor the applications opened, Data wrote in these applications, system connections, queries in the network and websites visited.
We use screenshots on certain intervals to see the information on screen and analyze the data with an AI, we see what applications are on screen at that time, what they have, what the user is doing, etc. 
## What we do with the Data
Now, this data is gathered basically in all the similar applications, but what we do with the information and what we create with it, it's what makes us different.
We search for "intent", "purpose" and "efficiency".
We use the AI to analyse organically, dynamically and contextualised each and every user activity, the AI analyses the data and as if it were a human, it can see what the user was intended to do, not only what he was doing, if the user was successful, if the action was efficient, and what could the user trying to do next.
This new data created with AI is useful and prevents the toxicity born from other apps that use simple algorithms that qualify something as white or black, and create general and not case specific "qualifications" for the user activity. 
These applications not only cause more harm than good, but also are easily by passed by the users when the algorithm is known.
### Example
If a user is inside YouTube, some may qualify it as a "bad" action by the user, specially on office working, but our Monitor asks "'Why is he in YouTube?", maybe he is looking a tutorial, maybe he is listening music to focus, maybe he is searching for alternatives, etc. And the AI understands that, uses it's "eyes" and data provided and makes that analysis, not ignoring what the user has been doing all along, to come to an understanding and a proper "qualification" for the user actions.

## Security and privacy concerns
This is an application that gathers the user activity and is capable of seeing the data in the screen, like a person could, but with abilities no normal person usually has, and this rises some security and privacy concerns.
### Privacy
The application MUST be user controllable, the user needs to be able to see what data is being shared, and be capable of removing this data from being shared if desired, with no restrictions.
Obviously, some companies may require a minimum data to be shared at working hours, and they may set up this in their devices, but the user is free to accept or reject this terms.
The application NEVER should store and/or share passwords, credit data, personal information (Like personal chats with family, couples, etc) and specially private forums or platforms that allow the complete anonymity of the users, as this would be illegal and a total violation to the other applications politics. 
### Security
The AI must be trained to identify personal information is the case some is shared, and Remove it before storing any RAG or Context.
Obviously, classic algorithmic solutions and security measures (Like to prevent the gathering of "password" inputs when Scrapping or getting the keyboard inputs) are to be put in order.
The place where this data is stored should allow to use client side databases, to prevent the cloud and possible data leaking. 
When using cloud storage, encryption should be the norm.

## Documentation structure

See [[INDEX.md]] for the full navigation guide.

Key documentation added:
- [[04-DATA-MODEL/MOC-Data-Model]] — Data structures and configuration schema
- [[05-INFERENCE-FRAMEWORK/MOC-Inference]] — Prompt architecture and context injection
- [[11-LIMITATIONS/MOC-Limitations]] — Known constraints and boundaries
- [[12-SCOPE/MOC-Scope]] — Project scope, capabilities, and excluded features
- [[13-USE-CASES-EXAMPLES/MOC-Use-Cases]] — Practical applications including RIWI
- [[DECISION-Installation-Approach]] — GUI vs local setup analysis
