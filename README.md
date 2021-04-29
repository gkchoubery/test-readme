# CDK Stack Overview

## Frontend Website Stack
- Creates an S3 bucket for frontend website deployment
- Creates a cloudfront distribution for S3 bucket
***
## Walmart Lister Stack

### Resources

#### <a name="lister-job-queue"></a> SQS: Lister Job Queue 
_Holds the data to process by [Lambda: Lister Actor](#lister-actor-lambda)_
- SQS: Lister Job Fail Queue (DLQ); 3 fails

#### Lambda: Sends messages to queue for jobs
_Gets pending jobs from database and push to [SQS: Lister Job Queue](#lister-job-queue)_
- Source -> CWEvent: (10 mins) - Invokes Lambda to send messages to SQS

#### <a name="#lister-actor-lambda"></a>Lambda: Executes lister jobs; Invoked by
- Source -> [SQS: Lister Job Queue](#lister-job-queue)
- Send messages to SQS: Lister Feed Polling queue

#### <a name="lister-status-update-queue"></a> SQS: Lister Status update queue
- SQS: Lister status update fail queue (DLQ); 5 fails
#### <a name="lister-polling-queue"></a> SQS: Lister Polling queue
- SQS: Lister feed polling fail queue (DLQ); 5 fails
#### <a name="bulk-processing-stream"></a>Stream: Bulk processing stream
- Bucket: Stores Bulk lister error files
- SQS: Bulk processor failed queue

***
## Walmart Inventory Stack

### Resources
