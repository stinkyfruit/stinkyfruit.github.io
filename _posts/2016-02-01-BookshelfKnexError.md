---
layout: post
title: Bookshelf/Knex Issue
---

Last week was a very trying week for my love for all-things databases. I'm using Bookshelf and Knex with MySQL. I ran into an error querying the database, that I could not, for the life of me, figure out. I had 4 super smart programmers look at my code and try to help me figure out what the issue was, but nobody could figure it out. Below is the code that everyone reviewed: 


```javascript
  editEvent: function(req, res){
    var data = req.body; 
    Event
    .where({id: data.eventId})
    .fetch({require: true})
    .then(function(event){
      return event.save({
        event_name: data.eventName || event.get('event_name'),
        event_date: data.date || event.get('event_date'),
        num_of_people_joined: data.numOfPeopleJoined || event.get('num_of_people_joined'),
        total_number_of_people_req: data.totalPeople || event.get('total_number_of_people_req'),
        price_per_person: data.pricePerPerson || event.get('price_per_person'),
        description: data.description || event.get('description'),
        image_url: data.image || event.get('image_url')
      }, {method: 'update'});
    }).then(function(){
      res.sendStatus(200);
    }).catch(function(error){
      console.log(error);
      res.send('Error at editEvent');
    });
  },
```

This should have worked. But I kept getting the following 2 errors:

Unhandled rejection Error: ER_DUP_ENTRY: Duplicate entry '1' for key 'PRIMARY'

I wasn't duplicating any keys, or changing any for that matter. Then I would change some code and get the following error:

>>Error: A model cannot be updated without a "where" clause or an idAttribute

I had a where clause! AND an ID attribute. What could it be? We tried so may things, over and over and over again:

We tried changing where to forge, we moved the order in which we were saving, we tried returning in different places, we tried using the keyword new, we tried only changing 1 thing, we tried saving nothing. And we were still getting the same errors. 

The whole time in the back of my head, I was thinking, maybe there is something wrong with my knex schema. Here is my schema for the events table:

```javascript
knex.schema.hasTable('events').then(function(exists){
  if(!exists) {
    knex.schema.createTable('events', function(events){
      events.increments('user_id').primary;
      events.string('event_name', 100);
      events.dateTime('event_date');
      events.integer('num_of_people_joined');
      events.integer('total_number_of_people_req');
      events.integer('price_per_person');
      events.string('description', 250);
      events.string('image_url');
      events.integer('creator').references('users.id');
      //.notNullable();
    }).then(function(table){
      console.log('Created Table', table);
    });
  }
});
```
Everything looks great right? Well... it actually wasn't. After 8 hrs of trying everything and talking to 5 people, I switched the primary column to read:

```javascript
events.increments();
```
instead of: 

```javascript
events.increments('user_id').primary;
```

From my understanding, knex will create this column and name it 'id'. My code above now works...

This simple change fixed the error that seemed impossible to fix. So what I gathered from all of this trial and error, knex and bookshelf require an id column for tables. If you decide to name the column yourself, be careful, because it won't read some naming conventions correctly, and you will run into errors.

For instance, 'uid' is ok, but 'user_id' is not. I believe bookshelf/knex looks for the key letters 'id', and stops when it sees a special character, such as an underscore; therefore, it was erroring out because it couldn't find 'id', which it reads as unique. 

It was a pretty fun lesson to learn, and I'm happy that I was finally able to figure it out, instead of scratching the entire schema. I still have a love for databases, despite how they feel about me.

![an image alt text]({{ site.baseurl }}/images/hard-work-quote-2.jpg "an image title")





