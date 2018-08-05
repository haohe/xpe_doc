## Micro Services

### Reading materials

1. https://github.com/haohe/xpe_doc/blob/master/ppt/microservices_20170602.pptx

### Notes

Micro service architecture is a special version of Service Oriented Architecture with additional architectural constraints:

1. A service must be sufficiently small to be maintained by a small team
2. A service should be self-contained 

When applying micro service architecture to web applications, it is important to note:

1. All business logics MUST be implemented on the server side as one or more micro services
2. All user interface (UI) features MUST be implemented on the client side using HTML5 technologies (HTML5 + CSS + JavaScript).
3. Interaction between UI and micro services are through RESTful interfaces or possibly Web Socket.

Things to avoid:

1. Avoid implementing UI on the server side.  Technologies such as PhP, JSP, ASP are now considered harmful. 
2. Avoid implementing business logics on the client side and that why the old fat client approach is bad. 


### Exercises

1. What is the fat client technology?
1. What is the difference between fat client and rich client technologies?
2. What is the dynamic web page technology?
3. Please give examples of UI features and business logics

### Questions and answers

1. Is using HTML5 technologies not SEO friendly?
