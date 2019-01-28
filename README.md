# Bitcoin Streaming Protocol

Stream data into the Bitcoin Blockchain.

# Intro

Create a potentially never-ending byte stream channel on the Bitcoin Blockchain.

# Protocol

The byte stream is divided into chunks small enough to fit into an OP_RETURN output. Each chunk can be of a different length. Each chunk is stored in a separate transaction.

To kinds of transactions exists.

## End of stream

The first output (index 0) is an OP_RETURN containing the last chunck of the stream. The payload is just a single pushdata. There is no prefix. The payload may be zero length.

## Append to stream

The transaction must have at least two outputs.

The first output (index 0) has a none zero value. The spending transaction of this output is the continuation of the stream. If the output is unspent the stream is open for appending. There is no restrictions on the script.

The second output (index 1) is an OP_RETURN appending data to the stream. he payload is just a single pushdata. There is no prefix. The payload may be zero length.

## Stream address

A stream is referenced by a transaction id and an optional offset into the data chunk.
