# Project Documentation: Wisper
## Table of Contents
1. [Introduction](#introduction)
2. [Problem We Solve](#problem-we-solve)
3. [How do we solve the problem](#how-do-we-solve-the-problem)
4. [Architecture](#architecture)
5. [Code Review](#code-review)
6. [Conclusion](#conclusion)

## 1. Introduction
Wisper is a p2p chatting application that enables to create chat rooms without needing a trusted chat application by creating a private local blockchain among users. Communication protocol can be Wi-Fi (even though there is no internet connection on the network) , Bluetooth or any other alternative. Every time a user joins a chat, they establish p2p connection by getting every node on the chain and introducing themselves to them and start communicating. Wisper doesnt store any data. Its all client side.


## 2. Problem We Solve
 Blockchains are only working with a proper internet connection, private blockchains are on the agenda, Adrian Brink, Cofounder of Anoma and Namada just made a speech about public networks are not that relieble when there is ongoing crisis such as war, disaster etc. There are lots of use cases for private local blockchains, and it they can solve so many problems if implemented. 

![enter image description here](https://pbs.twimg.com/media/GSW2YU4WAAEN5A-?format=jpg&name=medium)


Another aspect: In daily basis, centralized chatting applications such as whatsapp, telegram etc. are the most used applications. When it comes to chatting, we sometimes need privacy while exchanging private information. Whatsapp claims to do end to end encryption but there are no proofs because they are not open source. Even that is open-source, they have backend. Be honest, we all know "Meta" :) 

We enhance Ethereum's infra liveness by creating own little private chains by doing everything in the local chains and settlement to Ethereum at the end of the room.

## 3. How do we solve the problem?
Wisper is fully client side, so there are no middlemans that can access to messages, store them, sell them to companies. There are no requirement for "internet" connection.

For example, one of the use cases is in a company, board members can create a room and share it with the others. When it is created, a local, private, p2p chatroom is created. They can chat securely knowing there are nobody outside of the board has access to messages.

For the implementation we built at the hackathon, users downloads desktop application which is basically contains a node.js light-node implementation and React front-end. Wisper is fully open source and client-side. Since it is developed in a very clean way, anybody with a little coding or running node experience can verify this.

## 4. Architecture
![Architecture](https://i.imgur.com/Rvyhgt2.png)

This is the general architecture. Before breaking down the architecture, there is one big technology we use: **Desktop app that works on browser** . We developed our idea partially for the mobile app. Fully working demo is an desktop app built with electron, but client side parts working on browser with react app underneath. This enables to read all the code, and features that needs to connect a wallet with an extension is possible.

## **Node:**

![enter image description here](https://i.imgur.com/kwYCQmb.png)
This is the node deployed by the app. it contains three parts:
    **AddressBook:**
    In the node, we store lists of peers in the network. By doing this, users establish p2p connection between all of the other nodes in the chat. 
        **Tx:**
    Wisper stores the transactions(messages) in the node, so we can keep track of messages historically.When a new node participates in an existing room, they can not access the messages before since messages only stored locally.
    **Wallet:**
    Users need to sign the transaction they publish via gossip protocol. By doing this, users can be sure nobody can publish messages for another user. Every node who receive the message validate signature and sender_pubkey.

## **New node participating:**
![enter image description here](https://i.imgur.com/rspe2c4.png)
    When a new node joins the room, it fetches the existing addressBook from another node. After that new node communicates with the other nodes who are received. This ensures a fully decentrelized network protocol by giving the creator 0 priviledges. Other parties can continue to chat without needing creator
    **Consensus:**
    Actually, this blockchain does not require consensus between nodes, but we still need some verifications to ensure the received message sent by the owner of the address. So, before publishing the message user sign the TX, every node receives verify the signature.
	

## 5. Code Review
In this chapter, we will break down and demonstrate our project repo

**ui**
In that folder, we developed the client side with React. It communicates with the remaining network via its own localhost deployed by the electron app.

**controller**
Our main actions are in that folder, so it is enough to demonstrate it.

	Gossip

gossip folder contains the protocol needed for communication between nodes.Receive controller handles the logic of verify the data and signature and pass it to UI. Send controller handles the logic of creating,signing and sending a conversation

	peer

peer folder contains the protocol needed for introducing and recognizing  process between nodes. New user call introduce endpoint and it calls other node's recognize endpoint

	room

room folder contains the logic of creating a new room and joining an existing node.

## 6. Conclusion

In conclusion, Wisper represents a significant advancement in secure, decentralized peer-to-peer communication. By leveraging local blockchains and a client-side approach, it addresses critical issues present in centralized chat applications. Here are the key takeaways:

- **Privacy and Security**: Wisper ensures end-to-end encryption and eliminates the need for trust in centralized servers. Messages are stored locally and transmitted peer-to-peer, ensuring that only intended recipients can access them.
  
- **Decentralization**: Through the use of local blockchains, Wisper decentralizes communication networks. This architecture not only enhances reliability during crises but also empowers users with control over their data.

- **Accessibility and Transparency**: Being open-source and developed with user verification in mind, Wisper promotes transparency. Users can review the code and understand how their data is handled, fostering trust in the platform.

- **Implementation and Future Scope**: Initially developed as a desktop application with plans for mobile integration, Wisper demonstrates practicality and scalability. Future iterations could explore additional features and optimizations based on community feedback.

In essence, Wisper is more than just a chat application; it is a testament to the potential of decentralized technologies to reshape how we communicate securely and privately in an increasingly connected world.
