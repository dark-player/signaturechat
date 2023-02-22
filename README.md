<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>LE CHAT - Installation Guide</title>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
<meta name="description" content="LE CHAT - Perl CGI-script to set up your own webchat.">
<meta name="keywords" content="LE CHAT, lechat, chatscript, webchat, Perl, CGI, script, sourcecode">
<link rel="shortcut icon" type="image/gif" href="data:image/gif;base64,R0lGODlhEAAQALMAAP///wAAAEBAQICAgAAAgIAAgACAgMDAwICAgP8AAAD/AP//AAAA//8A/wD//////ywAAAAAEAAQAAAEPBAMQGW9dITB9cSVFoxBOH4SuWWbx5KTa5lrimKyDOp6KKK9FMx0Ew1ZF+Pp01PWmqoVpyaMHjOdbKcSAQA7">
</head>
<body alink="#ff4444" bgcolor="#ffddff" link="#4444ff" text="#000000" vlink="#9944ff">

<h1>LE CHAT Installation Guide</h1>

Last changed: 2018-01-06 for LE CHAT v2.0. Check the <a href="http://4fvfamdpoulu2nms.onion/lechat/">homepage</a> for any updates of this guide or the script!


<h2>Server Requirements</h2>
Your server or (free-)hosting account just needs to support Perl CGI scripts. Some of the CAPTCHA modules require support for the GD library though, but there are also modules that will work on any restricted server configuration even.

Read up on your server documentation how to enable Perl/CGI if it doesn't work already. On your own Apache server you can try adding those lines to your httpd.conf:
<br><br><table bgcolor="#F0F0F0"><tr><td><tt>
&lt;FilesMatch \.cgi$&gt;<br>
&nbsp;&nbsp;SetHandler cgi-script<br>
&lt;/FilesMatch&gt;
</tt></td></tr></table><br>

To install GD.pm on a Debian-based system you can try these terminal commands:
<br><br><table bgcolor="#F0F0F0"><tr><td><tt>
sudo apt-get install libgd-xpm-dev<br>
sudo apt-get install libgd-gd2-perl
</tt></td></tr></table><br>

On Windows you can use a current <a href="https://www.activestate.com/activeperl/downloads">ActivePerl</a> package, which has the GD binaries compiled in already. Windows should be used for local testing only, I wouldn't recommend it for a public server!

Make sure you read all the documentation and consider any security implications. Server security is a complex topic and you really must take it seriously before you should provide a public chat room. If you are a beginner, start with a private place and a few trusted friends only. Maybe invite a trusted tech guy who can check your server security first.


<h2>Edit the script file (optional)</h2>

There is just one line at the beginning of the script that you may want to change under certain circumstances.
<br><br><table bgcolor="#F0F0F0"><tr><td><tt>
my $datadir='./lcdat';
</tt></td></tr></table><br>

This is the data directory for all internal files of the chat, like members, sessions, etc. It's important that this directory is protected from web-access at all costs! The script tries to apply the correct chmod settings during initialisation already, but unfortunately this may have no effect on certain server configurations. If you run your own server, you may want to use something like '/var/lcdat' here. On a freehosting account that may not be possible, so make sure the directory will be protected from browser-access. The initialisation procedure will give you a possibility to check for that.


<h2>Upload lechat.cgi</h2>

This depends on your specific server setup, but most commonly you have to upload lechat.cgi into a special cgi-bin directory and change the chmod settings to 0711 (or maybe 0777 is needed) to make it executable. Check your server or (free-)hosting documentation for details. In rare cases it may even be needed to change the first line of the script to point to another Perl-interpreter.
In case you want to quickly reinstall an existing chat on another server, you can also upload a lechat.txt file that contains all the backup data that you want to restore on initialisation (members, config, language).


<h2>Upload the CAPTCHA modules (optional)</h2>

If your chat is private, you probably won't need CAPTCHAs to keep bots out (though some can still be fun to use), but a public chat is definately prone to automated attacks. For LE CHAT it doesn't matter where you put the module files, as long as the lechat.cgi script is able to read the files. The location will be given to the script in the superuser setup screen later. Just put the modules directory as is into a sensible location and, like the data directory, make sure it's not accessible from the web. For the StoredImages-CAPTCHA you should create a directory that <i>is</i> readable from the web. It'll be used to serve temporary image files to the user.<br><br>
As an example, I'll just assume that you put the directories from the zip-file as is in your web-root, the cgi-bin for the script files and the captchas directory for the temporary CAPTCHA images. Your directory structure will then look like this:

<br><br><table bgcolor="#F0F0F0"><tr><td><tt>
\---public_html<br>
����+---captchas<br>
����|�������index.html<br>
����|<br>
����\---cgi-bin<br>
��������|���lechat.cgi<br>
��������|<br>
��������\---modules<br>
������������|���generate.pl<br>
������������|���LeCaptchaExample.pm<br>
������������|���LeCaptchaImageRecognition.pm<br>
������������|���LeCaptchaMessageImage.pm<br>
������������|���LeCaptchaNumberGuess.pm<br>
������������|���LeCaptchaSecurityImage.pm<br>
������������|���LeCaptchaStoredImages.pm<br>
������������|���LeCaptchaWait120s.pm<br>
������������|<br>
������������+---fonts<br>
������������|�������StayPuft.ttf<br>
������������|<br>
������������+---generated<br>
������������+---ImageRecognition<br>
������������|���+---false<br>
������������|���|�������false.gif<br>
������������|���|<br>
������������|���\---true<br>
������������|�����������true.gif<br>
������������|<br>
������������\---StoredImages<br>
��������������������3wrPie.png<br>
��������������������SwgTx.png<br>
��������������������t6ub.png<br>
</tt></td></tr></table><br>

Some of the CAPTCHAs require additional Perl modules on your system, or your own customised settings and files to be useful. Read the comments in each <em>LeCaptcha*.pm</em>-file that you want to use, for further information!


<h2>Initialise The Chat</h2>

Call the script in your browser like this: <em>http://(server)/(cgi-path)/(script-name).cgi?action=setup</em> in our example that would be: <em>http://your.domain/cgi-bin/lechat.cgi?action=setup</em>
If you get a 500 Internal Server Error, then recheck the chmod settings and possible requirements for the shebang-line (#!/usr/bin/perl).

Once the script runs, you are guided through the setup process now, which should be pretty much self-explaining. Don't use your chatting nick for the superuser account or it will collide with the configuration setup later. Use a unique and secret nick instead. After you click "initialise chat" make sure you check the given links for the data directory and the admin file. If you don't get an error message (403 Forbidden), you must protect the directory yourself or edit the script file, as described above.<br><br>
Next proceed to the setup page and set the server directories correctly. If this is your only chat, then leave the data directory blank. If this will be an additional chat, e.g. a staff room or a room for a different language, where you want to use the same members, then fill in the path to the already existing data directory of the main chat. The directory can be absolute (e.g. <em>/var/lcdat</em>) or relative to lechat.cgi (e.g. <em>../staffroom/lcdat</em>).<br>
The modules directory can be absolute or relative and should be <em>./modules</em> in our example. The web directory for the temporary CAPTCHA images must be given relative to lechat.cgi and will be <em>../captchas</em> here.<br>
Click "save directory settings" to save your changes.<br><br>
Now register yourself a main admin nick or restore a members file backup of your chat, unless you have set a shared members file with your admin nick present already. Older members files are still fully functional with v2.0. From now on you can use <em>?action=setup</em> anytime to log in with your main admin nick to change the chat configuration. If you log in with your superuser nick you can still change the directories, your main admins or the language settings anytime.<br>
Click "log out" to go to the admin login.<br><br>
In case you translate the chat to a new language, please consider <a href="http://4fvfamdpoulu2nms.onion/feedback/">providing a copy</a> for everyone else! Thanks in advance. :-)


<h2>Change The Configuration Settings</h2>

Use <em>?action=setup</em> and log in with your main admin nick. You'll get access to all the chat configuration settings.<br>
First thing you should do: Save the page as *.html and keep it somewhere, in case you mess up your CSS and HTML settings at a later point. If anything goes wrong, you can always look up the default settings at least.<br><br>

Important: If there are multiple main admins in your chat, try not to edit the configuration at the same time. "save changes" will always overwrite the config file with your current settings on screen. This could become very confusing since your recent changes may mysteriously "disappear" if another admin is overwriting them at the same time. This issue will be addressed in a future version, though there should be no reason to edit the configuration very often by different admins.<br><br>


<h2>Further explanation of some settings:</h2>

<ul>
<li><b>Use external link redirection script</b>: If hotlinks are created, they will be redirected to a dereferrer page first to prevent leakage of the session-ID. If you also want to protect your (private) chat URL from leaking in the referrers, then you can setup another copy of this script on another server as a pure link redirector. For that, just set it to "link redirection only" at the top of the page in the Chat Setup.</li>
<li><b>Hours to remember guest logins</b>: The nicks and passwords of new guests will be remembered for this number of hours to preserve their names and protect against imposters. If the same guest doesn't login again during that time frame, the name is released and can be reused by someone else. If session and message expiration added together is bigger, then that value will be used, because there could still be messages of that guest in the room.</li>
<li><b>Number of guests from which on to hide entry/exit and shorten list</b>: If you have a lot of guests coming and going, the chatters list at the top of the messages and the entry/exit notifications can become very messy. If you set a value here, then the list of guests will be reduced to a number only and the entry/exit notifications omitted, once the number of guests in the room reaches that value.</li>
<li><b>Minimum time between posts from same nick (seconds)</b>: This is to prevent flooding of the chat. If a new message gets posted by the same nick before that time has passed, the message will be rejected. Only increase this value if it's really necessary.</li>
<li><b>CAPTCHA module to use</b>: Here all the modules will show up if you have set the directory correctly in the superuser setup. "Show splash screen before login" and "Use CAPTCHA on splash screen" have to be activated for a CAPTCHA to show.</li>
<li><b>Content filters</b>: If you are fluent in regular expressions, you'll love them. ;-) If not, check the page with <a href="http://4fvfamdpoulu2nms.onion/lechat/settings.html">helpful settings and content filters</a> for examples you can copy and paste.</li>
<li><b>Background colour, Text colour, Link colour, Visited link colour and Active link colour</b>: These are the default colours for the HTML &lt;body&gt;-tag. Even if you exclusively use CSS to style everything, make sure you set these colours according to your colour-scheme, since they are used for some internal HTML and CSS code and the readability check for randomised guest colours. Also browsers without CSS-support will be stuck with those colours for everything.</li>
<li><b>Any "CSS" fields</b>: Put your CSS there as you would inside the &lt;style&gt;&lt;!--...--&gt;&lt;/style&gt;-tags.</li>
<li><b>Any "style" fields</b>: Put your CSS there as you would inside the style="..." attribute of the element. Note: virtually every element has a class or id attribute, so you can just as well use the CSS-fields to style everything by its name. Examine the html-source to find the names.</li>
</ul>
See also <a href="http://4fvfamdpoulu2nms.onion/lechat/settings.html">Helpful Settings and Content Filters</a> for some more tweaks and ideas!


<h2>Make backups!</h2>

Sounds trivial, but don't forget it! When you have changed all the settings to your liking, then save a backup of the configuration. If you mess up or a server-fault erases the file, you can restore all the work you put in. Same with the members, always make a fresh backup after you have registered new members and store the text in a safe location.
Pro-Tip: Make a backup right at the beginning, before you even change anything. If you really screw up, you can then restore the basic default settings easily and start fresh.


<h2>Unsuspend The Chat Room</h2>

After you have saved your configuration, set the chat access to "enabled", so that you then can use the chat with the normal URL (without the <em>?action=setup</em>). Otherwise you'll just get a suspended error page.


<h2>Open The Room For Guests</h2>

By default the room is closed for all guests. Log in to the chat with your main admin nick and click the "Admin" button. There you can open the room for guests now.


<h2>Questions, Problems, Suggestions?</h2>

If you have any feedback, please <a href="http://4fvfamdpoulu2nms.onion/feedback/">let me know</a> and I'll update this guide as needed. A simple "Thank you!" is always appreciated as well. ;-)

<br><br><br>
<center><table width="650"><td bgcolor="#DDDDFF" align="center"><a href="http://4fvfamdpoulu2nms.onion/lechat/"><b><u>Back to LE CHAT</u></b></a></td></table></center>
</body>
</html>
