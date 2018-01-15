# What we want to persist

We don't know yet! PANIC!!!! :scream:

## Candidates:
### users
### tasks
### conversations
### messages

# How to store
## Middleware
Writing it middleware can't work because "storage middlewares can be used for storing attributes about a user or channel or team" unless we somehow extend this.

## Store objects

# How to retrieve

# What we want from persistence.
 - Not lose messages between reboots
 - Have a central data store that multiple docker containers can share when behind a load balancer without the need to pin conversations to a specific container
 - Have persistence work in multi threaded conversations


# Data store
 -  Redis
   * Why redis
        - Easy to set up and manage.
        - Botkit uses JS objects and arrays for everything so using a key value store is the path of least resistance.
        - There already exists [Redis Botkit middleware] we can borrow ideas from
        - High Read/Write speeds for quick retrieval of convos (important for load balancing)


# References
- [peterswimm on Conversation Persistence](https://github.com/howdyai/botkit/issues/506#issuecomment-262384273)

[Redis Botkit middleware]: https://github.com/howdyai/botkit-storage-redis
