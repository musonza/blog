---
title: "Implement the Commanding Pattern"
date: 2017-07-19
tags: ["PHP", "OOP", "DDD"]
draft: false
---

## What is the Command Pattern

Wikipedia definition:

>In object oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. 

The information that is encapsulated in a commanding object includes the method name, the object that owns the method and values for the method parameters. So in other words the commanding object will have enough information to carry out an action immediately or later on.

## Example Usage

Let’s take for example the method below that sends a message to other users in a conversation in a chat application.

<br/>

```php
/**
 * Sends a message
 *
 * @param Conversation $conversation
 * @param array $data contains message body and any other relevant information
 * @param User $sender
 *
 */
public function send($conversation, $data, $sender)
{
    //Step 1. Send a message by adding the $sender message to the $conversation
    
    //Step 2. Maybe we are sending a huge file and would rather que that for processing and let the other part of an incoming file

    //Step 3. Send a push notification to each user in the $conversation

    //Step 4. Send an email notification to each user in the $conversation

    //Step 5. Store notifications in the database for each user in the $conversation

    //Step 6. Tell the sender that their message has been sent
}
```

We could start coding our application and implement all the steps listed above in the `send()` function. However, by doing that we are well on our way to writing code that is not efficient and may not be easy to maintain. For instance, the is no reason why Step 4 above should wait for items like sending notifications. What if we can move all those steps somewhere else, that's where the Commanding pattern can help us. 

So let’s attempt to use the commanding pattern instead.

```php
<?php

namespace Musonza\Chat;

use Musonza\Chat\Commanding\CommandHandler;
use Musonza\Chat\Messages\SendMessageCommand;

class Chat {

    public function send($conversation, $data, $sender)
    {
        //$this->message refers to Message model
        $command = new SendMessageCommand($conversation, $this->message, $data, $sender);

        //CommandHandler
        $this->commandHandler->handle($command);
    }
    
}
```

In the function above we are encapsulating all information needed to send a message in the SendMessageCommand object.

Let’s take a look at the `SendMessageCommand` class: 

<br/>


#### <a name="SendMessageCommand"></a>SendMessageCommand

<br/>

```php
<?php

namespace Musonza\Chat\Messages;

use Musonza\Chat\Commanding\Command;

class SendMessageCommand implements Command
{
    public $sender;
    public $body;
    public $conversation;
    
    public function __construct($conversation, $message, $body, $senderId)
    {
        $this->conversation = $conversation;
        $this->message      = $message;
        $this->data         = $data;
        $this->sender       = $sender;
    }
    
    public function execute()
    {
        $this->message->send($this->conversation, $this->data, $this->sender);
    }
    
}
```

As you can see from the command constructor, we are setting the values for parameters that we need to perform our action of sending a message.

Below is our interface that all our commands will implement. That way we are sure each command has an `execute()` method.
```php
<?php

namespace Musonza\Chat\Commanding;

/** The Command interface */
interface Command {
   public function execute();
}
```

<br/>


#### <a name="CommandHandler"></a>CommandHandler

```php
<?php

namespace Musonza\Chat\Commanding;

/** The Invoker class */
class CommandHandler {
   public function handle($command) {
      //here we can store the command or do anything else
      $command.execute();
   }
}
```

## Summary

These are the steps we took in sending our chat message using the commanding pattern.

1. We made a call to the `send()` function with all the required information to send a message

2. We passed on that information to the Commanding object `SendMessageCommand`. 

  * Now the commanding object knows the information needed to send a new message
  * We can push this commanding object to a que for later execution or we can execute right away

3. We then made a call to [`CommandHandler`](#CommandHandler) method `handle()` which would cause the commanding object to call it's execution method. 

In our case when the execution method is called we send the message by making this call:

`$this->message->send($this->conversation, $this->data, $this->sender);`

<b>Note:</b> The code presented here is shortened. Feel free to <a href="https://github.com/musonza/chat" target="__blank">checkout a Laravel Package</a> I created that implements the commanding pattern.


