# queue producer

mission queue controller factory, it use for change mq service gently and unite the interface.

- ## Usage Example

    index.js

    ```javascript
    let Producer = require('queue-producer');
    Producer({
        redis: {
            url: 'redis://127.0.0.1:6379'
        },
        kafka: {
            url: '127.0.0.1:9602'
        },
        rabbit:{
            url: 'amqp://127.0.0.1:5672'
        }
    })

    ```

    logic.js

    ```javascript
    let producer = require('queue-producer');
    let queueInfo = {
        feedId: 'some id',
        userId: 'some id'
    };  
    producer.produce('queue tpye', queueInfo);
    ```

- ## API

- ### produce

    - #### Description

        produce queue mission

    - #### Arguments

        `type(String)` : a key identify the mission type,in redis is a key, in kafka is a topic

        `data(Any)`: mission data , it will be `JSON.stringify(data)` before produce

- #### currentQueueService

    - #### Description

        the producer public property, default is redis

    - #### Usage

        ```javascript
        //after initialize
        let producer = require('queue-producer');
        producer.currentQueueService = 'redis';
        // now it use redis to produce mission : redis.lpush
        Producer.currentQueueService = 'kafka';
        // now it use kafka to produce mission: new kafka.Producer().send
        ```

- ### init Option

    - #### Description

        init Queue mission

- #### Object Properties

    - `redis`: initialize redis option

        - `url`: redis url,support string or array (use for use for Consistent hashing)

        - `client`: redis client instance create by module: [node_redis](https://github.com/NodeRedis/node_redis)

        - `defaultKey`: default queue key

    - `kafka`: initialize kafka option (comming soon)

        - `url`: kafka url not zoomkeeper

  - `rabbit`: initialize rabbit mq option

    - `url`: rabbitmq url,it should have protocol such as `amqp[s]://[user:password@]hostname[:port][/vhost]`

    - `channel`: rabbitmq channel instance create by module: [amqplib](https://www.npmjs.com/package/amqplib)

  - `rocket`: initialize rocket mq option (not support yet)
