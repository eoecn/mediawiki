<?php
# GeshiHighlight.php
#
# By: E. Rogan Creswick (aka: Largos)
# largos@ciscavate.org
# ciscavate.org/wiki/
#
# Loosely based on SyntaxHighlight.php by Coffman, (www.wickle.com)
# Code arranged and packaged by Coffman (www.wickle.com) ;) 10-nov-2004
# Bug fix by Douglas McLaughlin (www.stricq.com) 9-Oct-2005
 
include('extensions/geshi/geshi.php');
 
global $lang;
 
class GeshiSyntaxSettings { };
 
$wgGeshiSyntaxSettings = new GeshiSyntaxSettings;
$wgExtensionFunctions[] = "wfGeshiSyntaxExtension";
 
 
function wfGeshiSyntaxExtension() {
 
  global $wgParser;
 
#  $langArray = array("actionscript","ada","apache","asm","asp","bash",
#                    "caddcl","cadlisp","c","cpp","css","delphi",
#                    "html4strict","java","javascript","lisp", "lua",
#                    "nsis","oobas","pascal","perl","php-brief","php",
#                    "python","qbasic","sql","vb","visualfoxpro","xml");
  
  $langArray = array( "actionscript-french",  "actionscript",     "ada",          "apache",
                      "applescript",          "asm",              "asp",          "bash",
                      "blitzbasic",           "caddcl",           "cadlisp",      "c_mac",
                      "c",                    "cpp",              "csharp",       "css",
                      "delphi",               "diff",             "dos",
                      "d",                    "eiffel",           "freebasic",    "gml",
                      "html4strict",          "ini",              "inno",         "java",
                      "javascript",           "lisp",             "lua",          "matlab",
                      "mpasm",                "mysql",            "nsis",         "objc",
                      "ocaml-brief",          "ocaml",            "oobas",        "oracle8",
                      "pascal",               "perl",             "php-brief",    "php",
                      "python",               "qbasic",           "ruby",         "scheme",
                      "sdlbasic",             "smarty",           "sql",          "vbnet",
                      "vb",                   "vhdl",             "visualfoxpro", "xml" );
 
  foreach ($langArray as $lang){
    $func = create_function('$text','$geshi = new GeSHi($text,"'.$lang.'","extensions/geshi/geshi/"); return
        $geshi->parse_code();');
# If GeSHI doesn't work then try commenting out the previous line and uncomment this next line to
# see the errors that GeSHI is encountering. This will output the error stream rather than the parsed stream
#       $geshi->error();');
    
    $wgParser->setHook($lang,$func);
  }
  
} 
?>
