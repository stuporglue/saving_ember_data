Saving Ember Data
=================

There are a couple of weird issue with saving related records, or possibly I don't know what I'm doing.

I'm trying to create an object (Bucket) which hasMany other objects (Records). The bucket is created first.
Later on a query is sent to retrieve some records. Before saving, I want to associate the records with the bucket then save these changes.

Issues
------

### Saving objects with  hasMany/belongsTo relationship loses the relationship

There is a [known bug](http://discuss.emberjs.com/t/ember-data-fixture-adapter-saving-record-loses-has-many-relationships/2821)
with the hasMany serializer (possibly only with the fixture adapter) but the fix found at the previous link doesn't completely fix the issue for me.

The fix makes it so that the bucket knows about the records, but the records (with the belongsTo) forget which bucket they belong to.

### .observes() and .observesBefore() on hasMany never triggered

I want these to be triggered whenever a new record is added to the bucket, but it doesn't happen. 

### setting belongsTo property doesn't mark record OR parent as dirty

[There](http://stackoverflow.com/questions/19998523/how-do-i-make-the-parent-dirty-when-i-change-a-hasmany-belongsto-relationship-in)
[are](https://github.com/ebryn/ember-model/pull/260)
[several](https://github.com/ebryn/ember-model/issues/157)
[tickets](https://github.com/emberjs/data/issues/866)
and threads out there about this, but I can't understand if the behavior is intentional (especially in the Fixture Adapter), a bug or if
I'm just doing something wrong.


Cases and Results
-----------------

This is what happens two simple scenarios. Each case is after the bucket has been created and saved.

The code is currently configured to use the serializer with the hasMany fix. You should be able to reproduce these results by opening the console then clicking button 1, then button 2.

I'm using ember 1.3.1 and ember-data 1.0.0-beta.5</p>

### Default Serializer

```
----------------Before set----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: 
    Bucket dirty: false
----------------After set----------------
    Records buckets: &lt;MF.Bucket:ember350:fixture-0&gt;
    Records dirty: false
    Bucket records ids: &lt;MF.Record:ember384:0&gt;
    Bucket dirty: false
----------------After record save----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: &lt;MF.Record:ember384:0&gt;
    Bucket dirty: false
----------------After bucket save----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: 
    Bucket dirty: false
```

### Serializer mentioned in hasMany bug report

```
----------------Before set----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: 
    Bucket dirty: false
----------------After set----------------
    Records buckets: &lt;MF.Bucket:ember350:fixture-0&gt;
    Records dirty: false
    Bucket records ids: &lt;MF.Record:ember389:0&gt;
    Bucket dirty: false
----------------After record save----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: &lt;MF.Record:ember389:0&gt;
    Bucket dirty: false
----------------After bucket save----------------
    Records buckets: 
    Records dirty: false
    Bucket records ids: &lt;MF.Record:ember389:0&gt;
    Bucket dirty: false
```

