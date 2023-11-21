# tapas-fs23-api
Defines the uniform HTTP API for the auction and ensures interoperability between groups.

## TODO:
1. Define constraints for each value. For example: What format must bidderName be?
2. Ensure that all relevant communication is specified
3. Should status updates be sent to original task?


## Service: tapas-auction-house

### Incoming Requests:

1. ReceiveBid 
   * auctionId
     * Identifies the specific auction currently bid on
   * bidderName
     * Represents the external entity's name in the format: "tapas-group1"
   * AuctionHouseUri
     * Used to interact with the auction house of the bidder
   * TaskListUri
     * To delegate the task to the winner (https://tapas-tasks.your-ip-adress.asse.scs.unisg.ch/)

### Outgoing Requests:

1. PlaceBid
   * *Same as body of ReceiveBid*

2. AuctionStarted -> published via MQTT/WebSub
   * publishing AuctionStartedEvent
   * Event consists of full Auction (see AuctionJsonRepresentation)
   * * Add new Parameters: AuctionFeedId & InputData
  

4. DelegateTask -> sent to bidderTaskListUri
   * Through AuctionWonEvent receive TaskList of winner (Is in the Bid).
   * Delegation of Task is normal Task Creation:
   * * Follows the TaskJsonRepresentation
   * Field OriginalTaskUri has to be correctly filled out
   * * This field is used by the external system to send patch updates on your task

## Service: tapas-tasks

### Incoming Requests:

1. ReceiveDelegatedTask -> received at bidderTaskListUri
   * Nothing should change since patch capabilites have been implemented by ronny
