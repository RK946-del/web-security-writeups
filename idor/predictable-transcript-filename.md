VULNERABILITY:
Chat transcript files are stored using predictable filenames and are accessible through static URLs without any object-level authorization checks, allowing attackers to retrieve files belonging to other users.

DEVELOPER ASSUMPTION:
The application assumes users will only access their own transcript files through the interface and will not attempt to manually modify URLs or guess other filenames.

WHY THIS IS DANGEROUS:
An attacker can enumerate predictable filenames (e.g., 1.txt, 2.txt, 3.txt) and directly request them via HTTP, allowing unauthorized access to other users' chat transcripts,files(images,pdf,reports etc) and sensitive information such as passwords.

DATA FLOW:
client->trasncript request->chats-> server-> 302 found/redirect-> client

EXPLOITATION STEPS:
1) visit the chat section start to chat.click view transcript
2) intercept the transcript request,observer the response gives 302 found with location url
3) enumerate predictable filenames and request the path with all types of request methods.
4) response:
    200 ok -> anauthroized access to other user's object
    403 forbidden -> proper implementation of object level authrization/idor failed
    405 Method not allowed -> the tried method is not allowed for this object or path


postswigger lab perfomed:https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references