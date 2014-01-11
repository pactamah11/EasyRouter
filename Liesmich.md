# EasyRouter

EasyRouter ist ein einfaches PHP Routing System.

- Schicke URI für jede Web- und Unterseite
- Suchmaschinenoptimiert
- Sehr einfach zu nutzen
- Schnell verbaut, in neuen, alten und aktuellen Projekten

### Installation

Zu Beginn muss die .htaccess Datei des Routing-Systems in das Root-Verzeichnis der Webseite kopiert/verschoben werden, in der es zum Einsatz kommen soll.

Nun starten wir das routen:

```php
<?php
	include('route.php');
  $base_directory = "http://www.yourwebsite.com";
  $_GET = $route->startRouting($base_directory);
?>
```

Die Parameter der URI sind nun in dem ```$_GET```-Array. Das wars auch schon, das Routing-System sollte nun funktionieren.  
Im folgenden Teil der Dokumentation stehen weiteren Optionen und Möglichkeiten, z.B. um den Parametern Namen zuzuweisen.

### Die Routing Function (startRouting();) und ihre Parameter.

```php
$route->startRouting( string $base_directory [, array $parameters = null [, array $exceptions = null ]] )
```

```$base_directory``` = URL zum Root-Verzeichnis der Webseite.

```$parameters``` = Namen der Parameter.

```$exceptions``` = Ausnahmen für Parameter. Der Parameter existiert am Ende nur, wenn ein Wert aus den Exceptions darin auftaucht.

#### Kleines Beispiel mit Parametern und Ausnahmen:

```php
<?php
	include('route.php');
    $base_directory = "http://www.yourwebsite.com";
    $parameters = array("language", "page"); // http://www.yourwebsite.com/language/page
    $exceptions = array();
    $exceptions[]= array(
    	"param" => "language",
        "exceptions" => array("en", "de") // /language MUSS en oder de sein, damit es in den Output-Parameter geladen wird!
    );
    $_GET = $route->startRouting($base_directory, $parameters, $exceptions);
?>
```

```
http://www.yourwebsite.com/en/user gibt folgendes aus:
$_GET['language'] = 'en'
$_GET['page'] = 'user'

http://www.yourwebsite.com/user/en gibt folgendes aus:
$_GET['page'] = 'user'
$_GET[1] = 'en'

http://www.yourwebsite.com/en gibt folgendes aus:
$_GET['language'] = 'en'

http://www.yourwebsite.com/user gibt folgendes aus:
$_GET['page'] = 'user'

http://www.yourwebsite.com/de/user?tab=contributions gibt folgendes aus:
$_GET['language'] = 'de'
$_GET['page'] = 'user'
$_GET['tab'] = 'contributions'
```

#### Der Optionen-Parameter

Der Optionen-Parameter transformiert den Wert in der URI, bevor er mit den Ausnahmen geprüft wird.

Beispiel:

```php
<?php
$basedir = "http://www.yourwebsite.com";
$params = array("page", "subpage");
$exceptions = array();
$exceptions[] = array(
	"param" => "page",
	"exceptions" => array("contributions", "hellostats", "commits"),
	"options" => array(
		"strtolower" => true, // Der Parameter page in der URI wird nun in Kleinbuchstaben umgewandelt -> z.B. StAtS wird nun zu stats
		"additionBefore" => "hello" // Und nun wird hello voran geschrieben -> stats wird nun zu hellostats
		# Nun, da keine Optionen mehr anfallen, wird geprüft, ob hellostats in den Exceptions steht
	)
);
$_GET = $route->startRouting($basedir, $params, $exceptions);
?>
```

```
http://www.yourwebsite.com/StAtS/test gibt folgendes aus:
$_GET["page"] = "hellostats"
$_GET["subpage"] = "test"

http://www.yourwebsite.com/contributions/test gibt folgendes aus:
$_GET["subpage"] = "contributions"
$_GET["1"] = "test"
```

Die Optionen sind:  
strtolower (bool)  
strtoupper (bool)  
strtotime (bool)  
strtoint (bool)  
inttobinary (bool)  
addition (string)  
additionBefore (string)  

```php
<?php
$exceptions[] = array(
	"param" => "param",
	"exceptions" => array("exception1", "exception2"),
	"options" => array(
		"strtolower" => false, // Standardwert ist false
		"strtoupper" => false,
		"strtotime" => false,
		"strtoint" => false,
		"inttobinary" => false,
		"addition" => null, // Standardwert ist null
		"additionBefore" => null
	)
);
?>
```

### Lizenz

The MIT License (MIT)

Copyright (c) 2014 EasyRouter

Permission is hereby granted, free of charge, to any person obtaining a copy  
of this software and associated documentation files (the "Software"), to deal  
in the Software without restriction, including without limitation the rights  
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell  
copies of the Software, and to permit persons to whom the Software is  
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in  
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR  
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,  
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE  
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER  
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,  
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN  
THE SOFTWARE.