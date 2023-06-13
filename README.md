# create-sqs-queues

Create SQS Queues and Redrive Policy if not exist

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
| `queueArn`             | The ARN of the main queue.             |
| `deadLetterQueueArn`   | The ARN of the dead-letter queue.      |

## Example Usage

```yaml
name: Create SQS Queues

on:
  push:
    branches:
      - main

jobs:
  create-queues:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Create SQS Queues and Redrive Policy
        id: sqs
        uses: path/to/create-sqs-queues@v1
        with:
          queueName: my-queue.fifo
          deadLetterQueueName: my-queue-dl.fifo
          queueRetentionPeriod: 259200
          maxReceiveCount: 4

      - name: Print Queue URLs and ARNs
        run: |
          echo "Main Queue URL: ${{ steps.sqs.outputs.queueUrl }}"
          echo "Dead-Letter Queue URL: ${{ steps.sqs.outputs.deadLetterQueueUrl }}"
          echo "Main Queue ARN: ${{ steps.sqs.outputs.queueArn }}"
          echo "Dead-Letter Queue ARN: ${{ steps.sqs.outputs.deadLetterQueueArn }}"
