---
title: "Fundstück: PHP Multi Dimensionales Array Nach Key Sortieren"
date: "2012-03-21"
categories: 
  - "allgemein"
---

Hallo,

ich bin heute auf einen sehr hübschen Code Schnipsel gestoßen. Mit Hilfe dieser Funktion lässt sich ein beliebiges Multi Dimensionales Array nach einem bestimmten Key sortieren. Funktioniert natürlich nur bei assoziativen Arrays :-)

Die Funktion:

\[php\] function array\_sort($array, $on, $order=SORT\_ASC) { $new\_array = array(); $sortable\_array = array();

if (count($array) > 0) { foreach ($array as $k => $v) { if (is\_array($v)) { foreach ($v as $k2 => $v2) { if ($k2 == $on) { $sortable\_array\[$k\] = $v2; } } } else { $sortable\_array\[$k\] = $v; } }

switch ($order) { case SORT\_ASC: asort($sortable\_array); break; case SORT\_DESC: arsort($sortable\_array); break; }

foreach ($sortable\_array as $k => $v) { $new\_array\[$k\] = $array\[$k\]; } }

return $new\_array; } \[/php\]

Beispiel:

\[php\] $values\[\] = array ("id" => "384f5de3ea2fded", sellerid => 1, sellername => "Zeppelin"); $values\[\] = array ("id" => "384f5de3ea2fdef", sellerid => 4, sellername => "Buxbaum"); $values\[\] = array ("id" => "384f5de3ea2fde1", sellerid => 3, sellername => "Rosenkranz"); $values\[\] = array ("id" => "384f5de3ea2fde2", sellerid => 7, sellername => "Lotus");

print\_r(array\_sort($values, "sellerid")); print\_r(array\_sort($values, "sellername")); \[/php\]
