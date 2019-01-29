# Bitcoin Streaming Protocol

Stream data into the Bitcoin Blockchain.

# Intro

Create a potentially never-ending byte stream channel on the Bitcoin Blockchain.

# Protocol

The byte stream is divided into chunks small enough to fit into an OP_RETURN output. Each chunk can be of a different length. Each chunk is stored in a separate transaction.

Two kinds of transactions exists.

## End of stream transaction

The first output (index 0) is an OP_RETURN containing the last chunck of the stream. The payload is just a single pushdata. There is no prefix. The payload may be zero length.

## Append to stream transaction

The transaction must have at least two outputs.

The first output (index 0) has a none zero value. The spending transaction of this output is the continuation of the stream. If the output is unspent the stream is open for appending. There is no restrictions on the script. If the spending transaction does not follow rules of this speficiation the stream is terminated with a read error.

The second output (index 1) is an OP_RETURN appending data to the stream. The payload is just a single pushdata. There is no prefix. The payload may be zero length.

## Stream address

A stream is referenced by a transaction id and an optional offset into the data chunk. How to encode this is out of scope for this protocol.

# Note

## No protocol prefix

This protocol only concerns writing to the stream. Discovery and naming of stream is out of scope. Thus it needs no prefix. 

## Forward reading only

Given a stream address it is not possible rewind to the start of the stream.

## Addresses are ignored

The stream is tracked by transaction id. Each append tranaction can use a different address. Write access handover can be done by appeding a zero length chunck to the stream. Multisig addresses can also be used.

## A stream never runs out of funds

Appending to a stream just requries that the first output is spent. Extra inputs can be added to provide the funds necessary to pay for the fee.


