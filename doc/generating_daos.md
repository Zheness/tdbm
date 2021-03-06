---
title: Generating DAOs and beans
subTitle: 
currentMenu: generating_daos
---

TDBM generates PHP files from your database model.

<div class="alert alert-info">Each time your database model is modified, you have to regenerate those files.</div>

The way you generate DAOs and beans **depends on the framework you use**.
 
<div class="row">
    <div class="col-xs-12 col-sm-6">
         <a href="install_laravel.html#generating-beans-and-daos" class="btn btn-primary btn-large btn-block">Generating DAOs and beans <strong>in Laravel</strong></a>
    </div>
    <div class="col-xs-12 col-sm-6">
         <a href="install_lumen.html#generating-beans-and-daos" class="btn btn-primary btn-large btn-block">Generating DAOs and beans <strong>in Lumen</strong></a>
    </div>
</div>

<br/>

<div class="row">
    <div class="col-xs-12 col-sm-6">
         <a href="install_silex.html#generating-daos-and-beans" class="btn btn-primary btn-large btn-block">Generating DAOs and beans <strong>in Silex</strong></a>
    </div>
    <div class="col-xs-12 col-sm-6">
         <a href="install_mouf.html#generating-daos-and-beans" class="btn btn-primary btn-large btn-block">Generating DAOs and beans <strong>in Mouf</strong></a>
    </div>
</div>

<br/>

<div class="row">
    <div class="col-xs-12 col-sm-6">
         <a href="manual_install.html#generating-daos-and-beans" class="btn btn-primary btn-large btn-block">Generating DAOs and beans <strong>in a manual install (no framework)</strong></a>
    </div>
</div>

<br/>

<div class="alert alert-info">
TDBM has a feature allowing you to access the database directly through the <code>TDBMService</code>, without generating DAOs or beans.
This is mostly for testing purpose and is not recommended. Should you need it, you can check the <code>tests/TDBMServiceTest.php</code> file for usage samples.</div>

The DAOs, beans, resultIterators structure
----------------------------

For each table in your database, TDBM will generate a DAO, a bean and a resultIterator. The DAO is the object you will use to
query the database and will return a bean or a resultIterator. Each row of the database will be mapped to a bean object.

DAOs, beans and resultIterators are divided in 2 parts. Let's assume you have a "users" table. TDBM will generate those classes for you:


- `AbstractUserDao`: the base class that contains methods to access the "users" table. It is generated by TDBM and you should
  never modify this class.
- `UserDao`: this class extends AbstractUserDao. If you have some custom requests, you should perform them in this class. You can
  edit it as TDBM will never overwrite it.
- `AbstractUser`: the bean mapping the columns of the "users" table. This class contains getters and setters for each and every
  column of the "users" table. It is generated by TDBM and you should
  never modify this class.
- `User`: this class extends AbstractUser. If you have some custom getters and setters, you should implement them in this class. You can
  edit it as TDBM will never overwrite it.
- `AbstractUserResultIterator`: the resultIterator representing a query on the "users" table. This class contains methods to specify you query.
  It is generated by TDBM and you should never modify this class.
- `UserResultIterator`: this class extends `AbstractUserResultIterator`. If you have some custom function to narrow down your queries you should implement them in this class. You can
  edit it as TDBM will never overwrite it.

Let's now have a closer look at the methods that are available in the "UserDao" class:

- `public function save(User $obj)` : saves a `User` object in database
- `public function findAll()` : returns all users records as an array of `User` objects.
- `public function getById($id, bool $lazyLoading = false)` : Get a `User` specified by its ID (its primary key)
- `public function delete($obj, $cascade=false)` : Deletes the User passed in parameter. If `$cascade` is set to true, it will delete all objects linked to `$obj`.

The last 2 functions are _protected_. It means they are designed to be used in the UserDao class.

- `protected function find($filter=null, $parameters = [], $orderbyBag=null)` : returns a list of
  users based on a filter bag (see the `TDBM_Service` documentation to learn more about filter bags). You can also
  provide an order, and an offset / limit range.
- `protected function findOne($filter=null, $parameters = [])` : this has exactly the same purpose as find except
  it returns only 1 bean object instead of a list of bean objects.
