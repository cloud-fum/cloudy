# Cloud Computing Project
An Introduction to Cloud Computing, Microservice Architecture, and Containerization concepts and applications.  

## Architect your application

You are required to propose a software solution based on Microservice architecture, document it all, and submit it for us.  

Choose your **base project**, an old monolith project you are familiar with, big enough to be broken apart into smaller services, forming a micro service, note that you will create a minimal functionality application for each service, for example, for an identity management service, login in, and registration is enough, in other word if your base project is much bigger, you can just create essential services and not use the rest of the monolith application.  
You can use existing codes you have written, the goal here is to **create more than 3 micro services**, in a minimal and simple way.  
You can propose a **Monolith** that you or your teammates has worked on, or you can find any open source existing Monolith project written in any language.  
Using the following projects is a **plus**:

* [https://github.com/bvn13/SpringBlog](https://github.com/bvn13/SpringBlog).  
* [https://github.com/citerus/dddsample-core](https://github.com/citerus/dddsample-core).  
* [https://github.com/mybatis/jpetstore-6](https://github.com/mybatis/jpetstore-6).  
 
You must link your Monolith and explain its functionalities and its file structure, and how it works.    
The process of turning a Monolith to a Micro service is very important, the candidate files and classes that are going to be a micro service must be documented, for example you must specify which portion of the Monolith files and classes are going to be Service A, and which are going to be Service B.  
It does not matter what framework or programming language you are using, in fact in this section you are not required to implement anything, you must write down explicitly your **reasons** for choosing these services, each service portion of code from Monolith, your plan for extraction of microservice from monolith project, your future tools of work including git, web frameworks or documentation tools, your team members and team work plans.  

*  Your Architecture must contain a user management service for issuing JWT tokens.  

JWT is used as an authorization token, it contains user information and is singed with server private key.  

![JWT_tokens_EN.png](./images/JWT_tokens_EN.png)

Interesting feature of a JWT token is that when a remote service has the private key of the issuer it can authenticate user identity by checking the signature of the JWT token. This means that your services must authenticate user identity by checking the signature of the JWT token.  

* Minimal functionality that works flawlessly is required, we want small and simple services, not a big and full featured application.  

* using diagrams is a good way to document your architecture, using google drawings is recommended.  

* Documenting your proposal in a markdown file is recommended, create a repository and put your docs there, create an issue and link your git repository here.  

> Note: A client side application is not counted as a service in this exercise. And you are not required to implement a client side application. Using Swagger is a minimum.    

* Using RESTFUL API is recommended for user endpoints.  

* No client side application is required, but suitable tools are required for testing, swagger is recommended.  

* You must design with at least **three microservice in action**, you **must use an existing monolith** project as your base, reduce its features if its too big, or increase its features if its too small. You can later use the old code base too.  

Consider the `Pooya` panel, for students and teachers and staff users, this is a gigantic project, and we have to break it down into smaller services. Our first service is `User management` for issuing JWT tokens and registering. The second service is `Course management` for creating courses and adding students to courses. The third service is `Student attendance management` for marking students attendance in each course. And the last service is `food management` for adding meals and buying food.  

![microservice pooa](https://docs.google.com/drawings/d/e/2PACX-1vQ888AxTtVLAa4Zl2BmdrNDhGP7cakfbABJoM0qLah2cRq4c71ilakEw_DwAjWrhJPUf_NX7J5Jib7B/pub?w=1279&h=2034)
<sub>*[access diagram](https://docs.google.com/drawings/d/1PF89Usn8z90N30fwD6SbfoFoWelLuc0EhWnR4t6Go8w/edit?usp=sharing)*</sub>  

As you can see this diagram is very top level and explains different services and user interactions.  

Each service **has its own database**, you must design your databases and add to your documentation(of course it may change in future but it is essential for you to know your data in each service). choose your services carefully as it may cause coupling between different services. consider `Course management` and `Student attendance management`, they must be synced on these two questions: 

* What are our courses?  
* Who is taking these courses?  

Think about it, if a teacher sends this request: `Attend True student id: 1, course id: 1`, how do the `Student attendance management` service know that the student is taking the course? or the course is real? the simplest way is to ask the `Course management` service every time with a message: `Hey course management, does this student: 1, taking this course: 1?`, or another way is to push changes in couser-student table to `Student attendance management` and replicate the database...  

![db pooa](https://docs.google.com/drawings/d/e/2PACX-1vTAHiq34UGoWSFmmwEox8Pmfuyq12HMcque8oziAnvnbIL6VoSu2D04VruIcWOTANyb-ggXrJF4SLRT/pub?w=1576&h=1046)
<sub>*[access diagram](https://docs.google.com/drawings/d/1D9_1H81qM_k5kU7j8H3UYpFJLCSdiShI6Hi8oNGeCjs/edit?usp=sharing)*</sub>  

Another example: 
![db sample](./images/dbsample.jpeg)

As you can see there are inevitable data dependencies between our services, especially on `user` table, with favor of JWT token we can just authorize users by the payload of JWT token, but in case of not using a JWT token, we had to contact the `user management` service for each user request and ask if the user is authorized or not, for example, if a student requests `Hi I am student with session id: X, can I take course: Y?` in `course management` service we have to ask `user management` service `Hi user management, is session id: X a valid user and is he a student?`.  

In a real Microservice with tens of services, you can imagine how complex it would be to communicate and synchronize data.   

![masters of chaos](./images/chaos.webp)

Highly recommend watching [this video on youtube](https://www.youtube.com/watch?v=CZ3wIuvmHeM) about Netflix Microservice Architecture.  

And in the last section, you must specify any internal call between your services, and specify the dependency that caused this action, also we are going to use GRPC to communicate.  

![design internal](https://docs.google.com/drawings/d/e/2PACX-1vTUpL4Eyl-XTZKNq84q92xZHcuOYEPADG1mQkzBuxCU-AjfdgIGWS_XOjDAQh9FyfmLOwxO5eQ4KGCo/pub?w=1415&h=799)
<sub>*[access diagram](https://docs.google.com/drawings/d/1HYA063pqDlEzRXHppd3D1u6ELBFjnc83aebRoWoSke8/edit?usp=sharing)*</sub>  

For example which piece of code in the Monolith made us to implement an internal GRPC call??  

* Your design must have at least one internal call.  

* Using GRPC is recommended, also you can reason using other options(except REST).  

