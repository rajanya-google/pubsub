### Introduction


### Building

These instructions assume you are using [Maven](https://maven.apache.org/).

1. If you want to build the connector from head, clone the repository, ensuring
    to do so recursively to pick up submodules:

    `git clone https://github.com/GoogleCloudPlatform/pubsub`

2. Unzip the source code if downloaded from the release version.

3. Go into the `test-prober` directory in the cloned repo.

4. Make the jar that contains the connector:

    `mvn package`

The resulting jar is at `target/pubsub-test-prober.jar`.

### Running the Prober

After building the prober, use the following command to run it:

`java -cp target/pubsub-test-prober.jar com.google.cloud.pubsub.prober.ProberStarter --project <project ID>`

If you want to use a service account for the test, first set the
`GOOGLE_APPLICATION_CREDENTIALS` environment variable:

`export GOOGLE_APPLICATION_CREDENTIALS=<path to json credentials file>`

When the prober starts up, it does the following:

1.  Deletes the topic and subscription provided if they already exist.

2.  Creates the topic and subscription provided.

3.  Sets up subscribers to receive messages.

4.  Continuously publishes messages.

### Prober Properties

The prober has many properties that can be set to test different load scenarios
and configurations. The following table details all the command-line options
that can be set.

| Property                                 | Value Type                 | Default                      | Description                                                                                                                  |
|------------------------------------------|----------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| project                                  | String                     | REQUIRED (No default)        | The project ID in which to create the topic and subscription.                                                                |
| endpoint                                 | String                     | "pubsub.googleapis.com:443"  | The Cloud Pub/Sub endpoint to send requests to.                                                                              |
| topic_name                               | String                     | REQUIRED (No default)        | The name of the topic that exists and publish messages to.                                                                   |
| subscription_name                        | String                     | REQUIRED (No default)        | The name of the subscription that exists and to receive messages from.                                                       |
| subscription_type                        | `STREAMING_PULL` or `PULL` | `STREAMING_PULL`             | The type of subscriber to create. See [subscriber documentation](https://cloud.google.com/pubsub/docs/pull) for differences. |
| message_failure_probability              | Double                     | 0.0                          | The probability with which a message should be nacked by the subscriber. Valid values are between 0.0 and 1.0.               |
| publish_frequency                        | Long                       | 1,000,000                    | The time between publishes in microseconds.                                                                                  |
| ack_delay_milliseconds                   | Integer                    | 0                            | The number of milliseconds by which subscribers should delay sending back acks or nacks.                                     |
| ack_deadline_seconds                     | Integer                    | 60                           | The ack deadline in seconds to use when creating the subscription.                                                           |
| thread_count                             | Integer                    | 8                            | The number of threads to use for processing delayed acks and nacks.                                                          |
| subscriber_count                         | Integer                    | 1                            | The number of subscribers to create on the subscription.                                                                     |
| pull_count                               | Integer                    | 10                           | When `subscription_type` is `PULL`, the number of pulls to do simultaneously.                                                |
| max_pull_messages                        | Integer                    | 100                          | When `subscription_type` is `PULL`, the maximum number of messages to request in each pull request.                          |
| subscriber_stream_count                  | Integer                    | 1                            | When `subscription_type` is `STREAMING_PULL`, the number of underlying streams to create per subscriber.                     |
| message_size                             | Integer                    | 100                          | The number of bytes per message. Set to <= 0 to generate randomly sized messages.                                            |
| message_filtered_probability             | Double                     | 0.0                          | The probability of a message being filtered out. Valid values are between 0.0 and 1.0.                                       |
| subscriber_max_outstanding_message_count | Long                       | 10,000                       | The maximum number of messages to allow to be outstanding to each subscriber.                                                |
| subscriber_max_outstanding_bytes         | Long                       | 1,000,000,000                | The maximum number of bytes to allow to be outstanding to each subscriber.                                                   |
