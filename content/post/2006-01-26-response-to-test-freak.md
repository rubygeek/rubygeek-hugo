---
categories:
- testing
- php
comments: true
date: "2006-01-26T00:00:00Z"
title: Response to Test Freak!
---
Note: this post was originally on my "PhpGirl" blog

Update: Chris Shiflett <a href="http://shiflett.org/archive/187">posted an informative response</a> on Test-More for PHP and testing in general

I started responding to the comment to my last post, then realized it was going to be long. So I put it in a post.

Thanks Damien Gilles, I have looked at that. Problem is, I have some resistance at work to using outside code. So I would need to work up a case for it beforehand and analyize it.

For really simple tests, this sort of testing (my friend Keith calls it Sanity testing) works fine. I don't really see the need to use a Unit Test framework. I have used PHPUnit before and it works fine, but ends up being just more typing (I think, but I'll try it and compare my results).

Here's an example of how I used it..
``` php
include_once("config.php");
include_once("User_Model.class.php");

$t = new Test_Simple(array("tests" => 20, "eol"=>" n"));

connect_db();
test_setup();

// Test object get/set methods
$user = new User_Model();
$user->setUserName("jdoe");
$user->setRealName("John Doe Test");
$user->setEmail("john@doe.com");
$user->setPermission("1");

// test true values
$t->ok( $user->getRealName()   == "John Doe Test", "get RealName" );
$t->ok( $user->getUserName()   == "jdoe", "get UserName" );
$t->ok( $user->getEmail()      == "john@doe.com", "get Email" );
$t->ok( $user->getPermission() == 1, "get Permission");

// test false values
$t->ok( $user->getRealName()   != "Susie Doe", "Not Realname Suzie Doe" );
$t->ok( $user->getUserName()   != "sdoe", "Not Username sdoe" );
$t->ok( $user->getEmail()      != "suz@doe.com", "Not email suz@doe.com" );
$t->ok( $user->getPermission() != 0, "Not Permission 0" );

// Save user
$saveResult = $user->save();
$t->ok( $saveResult == true, "Save was successful");

// Get ID of last user
$id = $user->id;

// clear object
$user = null;

// load user again and test again
$user = new User_Model($id);

// test true values
$t->ok( $user->getRealName()   == "John Doe Test", "get RealName after load" );
$t->ok( $user->getUserName()   == "jdoe", "get UserName after load" );
$t->ok( $user->getEmail()      == "john@doe.com", "get Email after load" );
$t->ok( $user->getPermission() == 1, "get Permission after load");

// change username
$user->setRealName("Mary Jane Test");
$user->setUserName("mjane");
$user->setEmail("mary@jane.com");
$user->setPermission("2");
$updateResult = $user->save();

// test true values
$t->ok( $updateResult == true, "Update was successful");
$t->ok( $user->id == $id, "ID stayed the same");
$t->ok( $user->getRealName()   == "Mary Jane Test", "get RealName after update" );
$t->ok( $user->getUserName()   == "mjane", "get UserName after update" );
$t->ok( $user->getEmail()      == "mary@jane.com", "get Email after update" );
$t->ok( $user->getPermission() == 2, "get Permission after update");

$deleteResult = $user->delete();
$t->ok( $deleteResult == true, "Delete was successful");

test_teardown();
$t->report();

// Clean up our mess before and after
function test_setup() {
connect_db();
$result = mysql_query("DELETE FROM users WHERE real_name like '%test%'");
}

function test_teardown() {
connect_db();
$result = mysql_query("DELETE FROM users WHERE real_name like '%test%'");
}
```

I did do a test_setup and test_teardown, similar to what you would have in a unit test. I only need to do this at the start of my "suite" and the end.

Of course, the main reason there's a test more (copied from Perl)  for php is so you can use Perl to run the test suites.  See this presentation by <a href="http://www.modperlcookbook.org/%7Egeoff/">Geoffrey Young</a> and <a href="http://shiflett.org/">Chris Shiflett</a> on <a href="http://www.modperlcookbook.org/%7Egeoff/slides/ApacheCon/2005/power-php-testing-printable.pdf.gz">Power PHP Testing(tgz file)</a> which, if you have a site with mixed PHP and Perl, would be a excellent way to test all your code with the same method.