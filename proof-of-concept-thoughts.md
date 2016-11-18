# Proof of Concept: Lessons Learned
Some takeaways from implementing the [*Spark* proof of concept](https://github.com/jdormit/spark-proof-of-concept).

## Separate the buffer abstraction from the editor abstraction
Buffers represent the actual text of a file. The buffer API should include functions to add or remove text at a particular point. Editors (or windows, or frames...) represent the interface onto the text. Their API includes functions to move the cursor and to capture keyboard input and commands. They call the buffer API. This separation of concerns is especially important since multiple editors can open into one buffer. Also - buffers and input are the core features of a text editor. Moving this out to an external library was a mistake that slowed down  development and limited my options (even though I am the author of the external library).

## Syncing the server and the clients
I didn't actually run into issues in my limited testing, but I'm concerned that when more clients connect or documents are more extensively edited the clients could get out of sync. To solve this, I can periodically overwrite the client buffers with the server buffer.

## Rendering the cursors
This ended up working really well, but due to my limited knowledge of Vue it took a little while. Basically, since I'm rendering cursors based on an object, I need to replace the whole object rather than just setting individual parts of it to ensure that Vue picks up the changes.

## Saving
I need to make sure the preventDefault() is called at the right time to avoid the browser save window from popping up.
