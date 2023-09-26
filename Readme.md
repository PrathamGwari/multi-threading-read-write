# Script README

## Overview

This script is designed to perform parallel data extraction from a MongoDB database and write the extracted data into separate JSON files using Node.js and the Node Cluster module for parallel processing.

## Dependencies

The script requires the following Node.js modules:

- `cluster`: For creating a cluster of worker threads.
- `os`: To determine the number of CPU cores available.
- `mongodb`: To connect to a MongoDB database.
- `fs`: For file system operations.
- `path`: To manage file paths.
- `JSONStream`: For JSON stream processing.

Install these dependencies using npm:

```bash
npm install cluster os mongodb fs path JSONStream

## Configuration

- `TOTAL_RECORDS`: The total number of records to be fetched from the MongoDB database.
- `batchSize`: The number of records to be fetched in each batch.
- `outputFolder`: The folder where JSON files will be written.

## Code Flow

1. Determine the number of CPU cores available.

2. In the master thread (cluster master):
   - Create a cluster of worker threads, one for each CPU core.
   - Assign an initial batch of records to each worker.
   - Workers communicate with the master through messages.

3. In worker threads (cluster workers):
   - Listen for messages from the master.
   - Establish a connection to the MongoDB database when a message is received.
   - Fetch a batch of records from the database, write them to a separate JSON file, and stream the JSON data to the file.
   - After completing the batch, send a message to the master indicating task completion and close the MongoDB connection.

4. The master thread keeps track of progress and assigns more work to available workers until all records are fetched.

## Execution

Run the script using the following command:

```bash
node <script-file-name.js>

## Output

Extracted data will be saved as separate JSON files in the `data` folder, with each file containing a batch of records from the MongoDB database.

## Error Handling

- Errors that occur during worker thread execution are logged to the console.
- If an error occurs in a worker, the script continues processing other workers and displays the error message.
- The script handles errors gracefully and ensures that all workers are terminated when the extraction process is complete.