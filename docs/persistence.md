# What we want to persist
Conversations, specifically conversations in tasks to take advantage of the `botkit.findConversation` function.

## Candidates:
### tasks
We don't want to store entire tasks but rather subsets of the conversations within tasks.

  * source_message - Object
  * id - Number


### conversations
Conversation objects are embedded in Task objects hence are not the primary resource to be stored.

Parts of conversations we want to store:
  * status - String ('active' or 'new')
  * messages - Array
  * thread -
  * threads -
  * startTime - DateTime
  * next_thread
  * processing - Bool (should be set to true)
  * id?
  * transcript (contains all the messages and can give the bot "some context" of the conversation)

### messages

Messages are in the Conversation objects

### users
User ids can be found within a Task instance.

# How to store
## Writing middleware
Writing it as middleware can't work because "storage middleware can be used for storing attributes about a user or channel or team" unless we somehow extend this.

## Storing javascript objects/key-value pairs
We want to store parts of conversations as they get added
(per message).

In this case we can write middleware to populate tasks in redis every time the Task mutated/updated.

# How to retrieve
Using the user's ID from Messenger as the hash key,
we will retrieve the corresponding data from Redis and populate Tasks at startup
with the persisted data that was stored.

# How to destroy
Listen for events of a terminated conversation with the user and delete the hash
from Redis.
This behaviour also happens when the conversation times out.


# What we want from persistence.
 - Maintain conversations between restarts
 - Have a central data store that multiple docker containers can share when behind a load balancer without the need to pin conversations to a specific container
 - Have persistence work in multi threaded conversations


# Data store
 -  Redis
   * Why redis
     - Easy to set up and manage.
     - Botkit uses JS objects and arrays for everything so using a key value store is the path of least resistance.
     - There already exists [Redis Botkit middleware] we can borrow ideas from
     - High Read/Write speeds for quick retrieval of convos (important for load balancing)

   * Schema
     - user_id (key in the Redis hash)
     -

# References
- [peterswimm on Conversation Persistence](https://github.com/howdyai/botkit/issues/506#issuecomment-262384273)

[Redis Botkit middleware]: https://github.com/howdyai/botkit-storage-redis
