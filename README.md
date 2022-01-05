# Replacer class

## Introduction

A PHP class to replace text elements.

The elements for replacements are kind of global replacements for the content
of your website, documentation, ...

The idea behind is to have shortcuts for snippets in your (html or md or ...) text.

You can 
* replace simple keywords with text (but for that you wouldn't need the class)
* cascade replacements
* use "parameters" in a replacement

The data what to to replace how can be read from multiple files to include
global replacements and more specific replacements for a project or a dependenncy.

This class can be useful in a parser or pre processor.

License: GNU GPL 3.0

## Example

```php

require_once __DIR__.'/../dist/replace-tags.class.php';

$oReplace=new axelhahn\ReplaceTags();
$oReplace->readTagfile(__DIR__.'/taglist.txt');
$oReplace->readTagfile(__DIR__.'/taglist_project.txt');

// example for adding replacements with a method
// $oReplace->addTag('__LICENSE__', 'GNU GPL 3');


$sSource=file_get_contents(__DIR__.'/example_content.txt');
echo "----- source: \n$sSource";


$sOut=$sSource;
$sOut=$oReplace->doReplace($sOut, 2);

echo "\n\n----- output: \n$sOut";

```

... where taglist.txt contains some replacements with or without placeholders

```
; ######################################################################
;
: TAGS
;
; Syntax:
;
;   [Tag] := [Replacement]
;   devider is always ":="
;
;   with arguments: use double quotes and introduce a variable with "$" 
;   character:
;   [Tag "$arg1[,..$argN]"] := [Replacement_with_$arg1...$argN]
;
;
; ######################################################################

[IMG "$img"]    := 

		<!-- image with alt text -->
		<br />
		<img src="$img" class="screenshot" alt="image $img"><br />

[IMGA "$img,$alt"]    := 

		<!-- image with alt text -->
		<br />
		<img src="$img" class="screenshot" alt="$alt"><br />

[VIDEO-YOUTUBE "$id"] := 
	<iframe 
		width="800" height="600" 
		src="https://www.youtube.com/embed/$id" 
		frameborder="0" allow="autoplay; encrypted-media" allowfullscreen
	></iframe>

__PRJ__             := ahreplace
__PRJNAME__         := ahReplace
__URLGIT__          := https://github.com/axelhahn/__PRJ__
__URLZIP__          := https://github.com/axelhahn/__PRJ__/archive/refs/heads/master.zip

```

The content file 

```
# Info zu meinem Projekt __PRJNAME__

Github  : __URLGIT__
Download: __URLZIP__

[IMG "testimage.png"]


And a test video: 
[VIDEO-YOUTUBE "iQZobAhgayA"]

```
