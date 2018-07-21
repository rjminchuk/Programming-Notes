# Todo
- write an article about records, dtos, and facades. 
- what are dtos?
   - Martin Fowler makes the argument that DTOs aren't great for every situation. https://martinfowler.com/bliki/LocalDTO.html
- where can you use them?
- present a simple problem
   - 
- present a simple architectural solution to hide complexity. A facade. 
- what’s the difference between an adapter and a facade?

# 2018.03.27 - DTOs, Facades, and Remote Interfaces

Working with remote interfaces can be painful. Have you ever dealt with an API that just .. fundamentally differs from your domain objects? At best, your contract is one to one. At worst, it's a composition of a summary of _most_ your domain objects. 

Let's talk about DTOs as what they are. They're classes, just like any other type in your code base. They're supposed to be lean, and they have a singular purpose. They are to help in communication with remote interfaces (_IE: APIs_), and by remote, I mean you can't change the interface. 

The DTO pattern is like the adapter pattern with remote interfaces. If an adapter is supposed to 