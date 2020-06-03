# Entry 6
##### 6/02/2020

# Summary

I lost motivation with the rhythm game and my partner vanished so I decided to turn a simpler project that I got paid to do into my freedom project. The entire project can be found [here](https://github.com/bytebin-xyz) although the frontend is not finished.

# Engineering Design Process

The simpler version of this project was just to create an API that allows file uploads from registered users. It was meant to be used for programs that allow you to take screenshots/screen recordings and have it automatically upload it to a file sharing site. An example of a program that does this is ShareX. The person wanted a replacement for the current ones that are already on Github as most of them are too simple and only allowed users with a password to upload onto the site. He wanted one where anyone can register an account to upload instead of having it password protected.

To turn this into my freedom project, I expanded on the idea further.

1. Users can create their own API keys with scopes to read/write files and user.

2. The API allows users to upload their files in chunks through many requests. This allows resumable uploads and chunk transformations before uploading it. It also allows the client to prevent re-uploading the entire file if a chunk failed to upload.

3. The frontend will allow users to encrypt their file with AES as it is being uploading (aka client side encryption as the original file contents never leaves the user's device).

At this moment, the frontend is not yet complete; only the login, logout, register is done. However the API is pretty much complete so it can be ran standalone without a frontend, but it is not currently hosted yet.

The API is built with [Nest](https://nestjs.com/) and uses [MongoDB](https://www.mongodb.com/) as the database.

The uploaded files are stored on the filesystem. I initially used GridFS, which is a specification to store files in MongoDB, however there are many problems with this.
  
  1. Database backups will become increasingly difficult
  
  2. Difficult to migrate file storage to another service (e.g. Amazon S3)
  
  3. The GridFS API in the mongodb driver didn't allow me to close the write stream without it deleting all chunks that were already uploaded. This means I have to keep the stream in memory until the upload has finished, which means memory might have been an issue since the garbage collector cannot free it yet.

# Skills

I learned about dependency injection, which is a design pattern used by the Nest framework.

# Next Steps

I need to finish the frontend and host it. This is one of my projects that I might keep hosted since it should be cheap enough to pay for it monthly.

[Previous](entry05.md) | [Next](entry07.md)

[Home](../README.md)
