---
categories:
- testing
- php
comments: true
date: "2006-01-25T00:00:00Z"
title: Test Freak!
---
Note: this post was originally on my "PhpGirl" blog

My two loyal readers know I like testing. Some say I'm sick.

I'm writing a PHP class for a user, and then think.. oh gee, how do I know if this works?? oh I'll write a Test::Simple for PHP. Yes, I know there exists one already that uses the power of Perl to test PHP files, but I didn't have time to figure out how to set that up and probably won't be able to use perl anyways on the production system. I can't stand to have global variables, so I made a class. Uber simple. There's probably better ways to do it (I can imagine something much more elegant in PHP 5). But, what do you expect on a whim and 40 minutes...

Code:
```php
// Nola's Wanna Be PHP Test Framework

class Test_Simple {
var $tests_to_run;
var $success_count;
var $failure_count;
var $test_count;

function Test_Simple($params) {
if (isset($_SERVER['HTTP_HOST'])) {
// we're on a server use a

$params['eol'] = "\ n "; //take out spaces, had to have them for this blog
}
$this->tests_to_run = $params["tests"];
$this->eol =  empty($params['eol']) ? "\n" : $params['eol'] ;
$this->success_count = 0;
$this->failure_count = 0;
$this->test_count = 0;
print "1..{$this->tests_to_run}{$this->eol}";
}

function ok ($expression, $message) {
$this->test_count++;
if ($expression == false) {
print "not ";
$this->failure_count++;
} else {
$this->success_count++;
}
print "ok {$this->test_count} - $message" . $this->eol;
}

function report() {
print $this->eol;
if ($this->test_count tests_to_run) {
print "You ran {$this->success_count} out of "
. "{$this->tests_to_run} tests " . $this->eol;
} else {
print "Opps, looks like you meant to run {$this->tests_to_run} "
. "tests but ran {$this->test_count} instead" .$this->eol;
}
print "Success Rate: "
. (($this->success_count *100)/ $this->test_count)
. "%".$this->eol;
}

}
```
And.. I made some code to test itself..
```php
$t = new Test_Simple(array("tests" => "4"));
$t->ok($t->test_count == 1, "one test run");
$t->ok($t->tests_to_run == 4, "running 4 tests");
$t->ok($t->eol == "\n", 'end of line is \n');
$t->ok(true, "its true!");
$t->ok(!false, "its not false");

$t->report();
```
I will probably make improvements upon this... suggestions? .. my test "report" is a bit more verbose than the standard Test::* in Perl.

I made it so you can run it with the php executable ... or with the web browser.

I tried to install Perl's class <a href="http://search.cpan.org/%7Epgollucci/Apache-Test-1.27/lib/Apache/TestRunPHP.pm">Apache::TestRunPHP</a> but had a few problems getting it setup on windows. I think I need mod_perl installed on my Apache. I did find out that it takes a different approach than my simple class, and starts apache, runs tests, prints report and shuts down apache. Interesting. My tests I only intended to run individually (actually didn't know how TestRunPHP worked till after I wrote my class).