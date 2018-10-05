
# Richardson Maturity Model

# Level 0 

- Expose SOAP web services in REST style. 
- http://server/getPosts, http://server/deletePosts, http://server/doThis, http://server/doThat etc using REST.


# Level 1 

- Expose Resources with proper URI’s (using nouns). 
- http://server/accounts, http://server/accounts/10. 
- HTTP Methods are not used.


# Level 2 

- Resources use proper URI’s + HTTP Methods. 
- For example, to update an account, you do a PUT to . 
- The create an account, you do a POST to . 
- Uri’s look like posts/1/comments/5 and accounts/1/friends/1.


# Level 3 

- HATEOAS (Hypermedia as the engine of application state). 
- You will tell not only about the information being requested but also about the next possible actions that the service consumer can do. 
- When requesting information about a facebook user, a REST service can return user details along with information about how to get his recent posts, how to get his recent comments and how to retrieve his friend’s list.
