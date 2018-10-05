
# Richardson Maturity Model

# Level 0 

- Expose SOAP web services in REST style. 
- http://server/getPosts, http://server/deletePosts, http://server/doThis, http://server/doThat etc using REST.


# Level 1 

- Expose Resources with proper URI’s (using nouns). 
- http://server/accounts, http://server/accounts/10. 
- HTTP Methods are not used.
- POST - Create a new resource
- GET - Read a resource
- PUT - Update an existing resource (complete replacement)
- PATCH - Partial update
- DELETE - Delete a resource


# Level 2 

- Resources use proper URI’s + HTTP Methods. 
- For example, to update an account, you do a PUT to . 
- The create an account, you do a POST to . 
- Uri’s look like posts/1/comments/5 and accounts/1/friends/1.


# Level 3 

- HATEOAS (Hypermedia as the engine of application state). 
- You will tell not only about the information being requested but also about the next possible actions that the service consumer can do. 
- When requesting information about a facebook user, a REST service can return user details along with information about how to get his recent posts, how to get his recent comments and how to retrieve his friend’s list.

# Configuration

```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-hateoas</artifactId>
    </dependency>
```

```java
@GetMapping("/students/{id}")
public Resource<Student> retrieveStudent(@PathVariable long id) {
  Optional<Student> student = studentRepository.findById(id);

  if (!student.isPresent())
    throw new StudentNotFoundException("id-" + id);

  Resource<Student> resource = new Resource<Student>(student.get());

  ControllerLinkBuilder linkTo = linkTo(methodOn(this.getClass()).retrieveAllStudents());

  resource.add(linkTo.withRel("all-students"));

  return resource;
}
```

```json
{
  "id": 10001,
  "name": "Ranga",
  "passportNumber": "E1234567",
  "_links": {
    "all-students": {
      "href": "http://localhost:8080/students"
    }
  }
```



