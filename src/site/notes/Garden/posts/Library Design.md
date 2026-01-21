---
{"dg-publish":true,"permalink":"/garden/posts/library-design/","created":"2026-01-21T14:50:55+08:00","updated":"2026-01-21 14:50"}
---


# Library Design

How to design a programming library for other projects to import and use?

## Design Considerations
- library should take care of itself
    - if a library needs to interact with DB, then it should be the one initiating db connection
- library should be generic to support many use cases
- library should allow users to be able to override certain things
- library should not assume things to be done in specific way
    - e.g. it should not assume there will be `.env` file with the variables it requires
- library must specify exactly what the user needs to configure, needs to know
- library should automate/abstract out as many things as possible for the user
- library must have extensive testing to ensure functionalities are working as intended
- library must be backward compatible at least a few versions
- library cannot remove functions without showing deprecation warning a few versions before. 

