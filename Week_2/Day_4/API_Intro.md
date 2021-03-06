# API Intro

* Application Programming Interfaces (APIs) are sets of requirements that govern how one application can talk to another.

* APIs on the web allow big services like Google Maps or Facebook to let other apps "piggyback" on their offerings.

* APIs limit outside programs access to a specific set of features - often requests for one data set or another.

* REST API (REpresentational State Transfer) - works pretty much the same way as a website does.
  * Client makes a call to a website using HTTP

  * We can request the JSON from a website
    * We can further ask for specific key values in the request
    ```
    <url>/fields=id,names,likes
    
    facebook.com/profile=123123
    ```
     