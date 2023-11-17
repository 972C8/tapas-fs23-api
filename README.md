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
   * bidderAuctionHouseUri
     * Used to interact with the auction house of the bidder
   * bidderTaskListUri
     * To delegate the task to the winner

### Outgoing Requests:

1. PlaceBid
   * *Same as body of ReceiveBid*

2. AuctionStarted -> published via MQTT/WebSub
    * auctionId
      * The id is created here and published for other parties to bid
    * taskType
      * For other parties to know whether they have available executors

3. DelegateTask -> sent to bidderTaskListUri
    * taskName
      * Mandatory field to create a new task. Should be the auctionHouseUri
    * taskUri
      * Taken from the auction object and represents the original task (where the response it sent back to)
    * taskType
      * The task type for which an executor is needed
    * inputData
      * The data to be processed by the external executor

## Service: tapas-tasks

### Incoming Requests:

1. ReceiveDelegatedTask -> received at bidderTaskListUri
   * *Same as body of DelegateTask*
   * **RESULT:** is sent via PATCH directly to taskUri with OutputData