# libcast/highrise

This library is a full rewrite of [Highrise-PHP-Api](https://github.com/ignaciovazquez/Highrise-PHP-Api) aiming to improve usability and stick with modern PHP practices.

New features include:

* PHP 5.3 namespaces
* Composer support
* Ability to list global subject fields
* Ability to add custom fields to a Person
* Ability of deleting a email address of a Person

Existing features in original library:

* People
* Tasks
* Notes
* Emails
* Tags
* Users


## Installation

The recommended way to install `libcast/highrise` is to use [Composer](https://getcomposer.org):

```bash
$ composer require libcast/highrise
```


## Usage

### People

Create a new person and set his address:
```php
use Highrise\Resources\HighrisePerson;

$person = new HighrisePerson($highrise);
$person->setFirstName("John");
$person->setLastName("Doe");
$person->addEmailAddress("johndoe@gmail.com");

$address = new HighriseAddress();
$address->setAddress("165 Test St.");
$address->setCity("Glasgow");
$address->setCountry("Scotland");
$address->setZip("GL1");
$person->addAddress($address);

$person->save();
```

Find people by search term:
```php
$people = $highrise->findPeopleBySearchTerm("John");
foreach($people as $p) {
    print $person->getFirstName() . "\n";
}
```

### Notes

Print all notes:

```php
foreach($highrise->findAllPeople() as $person) {
    print_r($person->getNotes());
}
```

Create a new note:

```php
$note = new HighriseNote($highrise);
$note->setSubjectType("Party");
$note->setSubjectId($person->getId());
$note->setBody("Test");
$note->save();
```

### Tags

Add tags:

```php
$people = $highrise->findPeopleByTitle("CEO");
foreach($people as $person) {
    $person->addTag("CEO");
    $person->save();
}
```php

Remove Tags:

```php
$people = $highrise->findPeopleByTitle("Ex-CEO");
foreach($people as $person) {
    unset($person->tags['CEO']);
    $person->save();
}
```

Find all tags:

```php
$all_tags = $highrise->findAllTags();
print_r($all_tags);
```

### Tasks

Create task:

```php
$task = new HighriseTask($highrise);
$task->setBody("Task Body");
$task->setPublic(false);
$task->setFrame("Tomorrow");
$task->save();
```

Assign all upcoming tasks:

```php
$users = $highrise->findAllUsers();
$user = $users[0]; // just select the first user

foreach($highrise->findUpcomingTasks() as $task) {
    $task->assignToUser($user);
    $task->save();
}
```php

Find all assigned tasks:

```php
$assigned_tasks = $highrise->findAssignedTasks();
print_r($assigned_tasks);
```

### Users

Find all users:

```php
$users = $highrise->findAllUsers();
print_r($users);
```

Find current user:

```php
$me = $highrise->findMe();
```
