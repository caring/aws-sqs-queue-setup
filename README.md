# create-sqs-queues

Create SQS Queues and Redrive Policy

## Description

This GitHub Action creates two Amazon Simple Queue Service (SQS) queues: a main queue and a dead-letter queue. It also configures a redrive policy for the main queue to handle messages that exceed the maximum receive count.

## Inputs

| Name                  | Description                                                 | Required | Default         |
| --------------------- | ----------------------------------------------------------- | -------- | --------------- |
| `queueName`           | The name of the main queue.                                 | ✔        | `my-queue.fifo` |
| `deadLetterQueueName` | The name of the dead-letter queue.                          | ✔        | `my-queue-dl.fifo` |
| `queueRetentionPeriod` | The message retention period for the main queue in seconds. |          | `259200` (3 days) |
| `maxReceiveCount`     | The maximum number of times a message is received from the main queue before being moved to the dead-letter queue. |          | `4`             |

## Outputs

| Name                   | Description                           |
| ---------------------- | ------------------------------------- |
| `queueUrl`             | The URL of the main queue.             |
| `deadLetterQueueUrl`   | The URL of the dead-letter queue.      |
| `queueName`             | The name of the main queue.             |
| `deadLetterQueueName`   | The name of the dead-letter queue.      |

## Example Usage

```yaml

jobs:
  create-queues:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Create SQS Queues and Redrive Policy
        id: sqs
        uses: caring/provision-aws-sqs-queue@v1.0.1
        with:
          queueName: my-queue.fifo
          deadLetterQueueName: my-queue-dl.fifo
          queueRetentionPeriod: 259200
          maxReceiveCount: 4

      - name: Print Queue URLs and Names
        run: |
          echo "Main Queue URL: ${{ steps.sqs.outputs.queueUrl }}"
          echo "Dead-Letter Queue URL: ${{ steps.sqs.outputs.deadLetterQueueUrl }}"
          echo "Main Queue Name: ${{ steps.sqs.outputs.queueName }}"
          echo "Dead-Letter Queue Name: ${{ steps.sqs.outputs.deadLetterQueueName }}"
```

This action will create the specified SQS queues if they do not already exist and configure the redrive policy for the main queue. The action outputs the URLs and names of the created queues, which can be used in subsequent steps of your workflow.

## License

This project is licensed under the [MIT License](LICENSE).

## Author

Written by the Devops Team.