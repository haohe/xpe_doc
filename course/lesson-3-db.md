# XPE DB

You need to complete [Tutorial 4 ](../tutorials/tutorial_4.md) before this lesson.

You also need to run the latest XPE image: xpedocker/xpe:latest

Detailed instructions can be found at https://hub.docker.com/r/xpedocker/xpe/

Once you have the web IDE running on your local host, go to the IDE and clone the students_db project from:

https://github.com/softtouchit/students_db.git


## Simple server-side DB validators

To enable the server side validation, we can add validators to each field.  Let's consider the dictionary of the studnets db:

```xml
dict="id,studentNo:s!,name:!,age:i,sex:!,created:t"
```

The id field and created fields are populated by the server automatically.  StudentNo, name, age and sex are supplied by users.  

First, we don't want StudentNo to be empty and all the values should be at least 4 characters long and 8 characters maximum, we theorefore add the following validators attribute.


```xml
     <service store="students.db" storeType="binary" primaryKey="id" fields="id,studentNo,name" dict="id,studentNo:s!,name:!,age:i,sex:!,created:t" seqKey="true" validators="studentNo:size(4,8)">
```

Similarlly, we want the name to be at least 2 characters long, and 32 characters maximum.  We also want the age to be a number.  We also want sex to be either 'M' or 'F'.

```xml
     <service store="students.db" storeType="binary" primaryKey="id" fields="id,studentNo,name" dict="id,studentNo:s!,name:!,age:i,sex:!,created:t" seqKey="true" validators="studentNo:size(4,8),name:size(2,32),age:number(),sex:enum(M,F)">
```

## XSS attack

When storing data into DB, attackers can put JavaScript code into the DB. When displayed, this code a secretly leak a user's data to others.  To prevent this from happening, one needs to do two things:

1. Ensure content is checked to be safe before storing it.
2. Ensure content is safe before showing it from the DB on the client side.

The first part can be easily done with the safehtml() validator, so our example becomes:


```xml
     <service store="students.db" storeType="binary" primaryKey="id" fields="id,studentNo,name" dict="id,studentNo:s!,name:!,age:i,sex:!,created:t" seqKey="true" validators="studentNo:size(4,8),name:size(2,32),age:number(),sex:enum(M,F),*:safehtml()">
```
