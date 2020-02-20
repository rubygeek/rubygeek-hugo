---
categories: perl
comments: true
date: "2007-04-12T00:00:00Z"
title: Right Tool for the Right Job
---
On one of my projects the requirements was to take two csv files, one with words and data and one with words. The goal is to use the list of words to retrieve the data from the other. Long-term storage for the datasets is not needed, so I didn't see a reason to put in a database. Typically the language used for this company is PHP (though I have sneaked in a few ruby scripts.. shhhh) so my first try was to load each file into an array in memory and do a lookup. It worked fine for my sample files but croaked when I fed it the real data. My real data file was 180k some words and my word list was 400k words. So, I tried using a temporary table. I started it when I left for the day and it was still going the next morning. This was unbearable slow and never actually completed. I could have probably tried to optimize or choose different table types, but I didn't...because I was thinking hmmmmm... wonder if Perl will be faster? It was, not only did it complete --- but finished in minutes (with printing each word out to the display) or in seconds (in a more silent mode). I showed the people who would be using it – they were amazed. This sort of thing takes hours in excel and crashes sometimes. 

``` perl
# load data list into hash
$parser = Parse::CSV->new( binary => 1, file => $infile_name );
my %DATA = {};
print "\n\nNow indexing data file\n";
while ( $line = $parser->fetch ) {
	next if !$line;
	$word = shift @$line;	
	next if ( (!defined($word)) or ($word eq ''));
	$DATA{$word} = $line;
	print $verbose ? "Indexing $word\n" : '.';
} 
```

The files sometimes have foreign characters in it so I open it in binary mode. <a href="http://search.cpan.org/~adamk/Parse-CSV/">Parse::CSV</a> is a wrapper for <a href="http://search.cpan.org/~jwied/Text-CSV_XS/">Text::CSV_XS</a> class and is much easier to use. Its written by <a href="http://ali.as/">Adam Kennedy</a> and I had a question about it and was able to find him in IRC and ask him myself, and got some help from someone else in the room too -- pretty cool! Back to the code, I skip if the line is not defined, shift off the first element of the array. The fetch method always returns an array reference, so I must prefix it with @ to access it as an array and shift off the first element. If thats not defined or blank, I use as the key to store the reference to the array. If verbose option was used, I display a longer notification to the user, otherwise I display just a period. That way the user knows its working! 

``` perl
#setup excel file
my $workbook = Spreadsheet::WriteExcel->new($outfile_name);
my $worksheet = $workbook->add_worksheet();

#process word list

$parser = Parse::CSV->new( binary => 1, file => $wordfile_name );
my $row_count = 0;
my (@row, $row_ref);
print "\nNow starting lookup...\n";
while ( $line = $parser->fetch ) {
	@row = ();
	$word = shift @$line;
	next if ( (!defined($word)) or ($word eq ''));

	if (exists $DATA{$word}) {
		#word found
		$row_ref = $DATA{$word};  #get ref to array 
	  	print $verbose ? "Found $word\n" : '+';
		push @row, $word;
		push @row, @$row_ref;
		$worksheet->write_row($row_count, 0, \@row);
	} else {
	  	print $verbose ? "Not Found $word\n" : '-';
		$worksheet->write_row($row_count, 0, [$word, 'not found']);
	}
	$row_count++;
}
```

Similar to the loading of the hash, I open the file with Parser::CSV as before. I initialize my variables and set a row_count used to write to the excel file. This data file is supposed to be a single column list of words, so I could probably get away with just doing a simple file read but just in case I treat as a CSV file. Once I get the word variable, I see if it exists as a key in my %DATA hash. <em>Funny thing about Perl regarding hashes and arrays – when referring to the whole thing you use % for hash, @ for array. But when referring to a particular element, you use $. Kinda funny but in a way makes sense...hehe.</em> :P I simplified this code a bit so you get the concept and these lines better no doubt. I used @row as a temporary holder for the data I want to write to the excel file, I had some other code here I took out which had more to do with the @row. Finally I write the row array to the the excel file. The method requires an array ref, and \@row gives me a reference. 

Thats basically it. The loading of %DATA hash is of course, dependent on how much memory you have. If you have problems you could try tieing it to a DB file:

```
use DB_File;

tie %DATA, 'DB_File', 'output.db' or die ('cannot open output.db');
```

Building a DB file also has the advantage of persistence. If you need to run a lot of lookups, multiple times you may be interested in using it. It will be slower to some extent, but it will work! :)

My script could probably be optimized some and perhaps <a href="http://en.wikipedia.org/wiki/Perl#Perl_golf">golfed</a> some which could make it faster. But I try and write Perl in english, at least for the first pass. Then perhaps I can use the shortcuts and see if it runs faster. This was a fun project and I got to learn more Perl. My friends <a href="http://www.devchix.com/2006/09/18/allow-me-to-introduce-myself/">Liz</a> and Yaakov helped me and I thank them :)
