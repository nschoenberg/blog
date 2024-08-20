---
title: "Fundstück: PHP Multi Dimensionales Array Nach Key Sortieren"
date: "2012-03-21"
categories: 
  - "allgemein"
---

Hallo,

ich bin heute auf einen sehr hübschen Code Schnipsel gestoßen. Mit Hilfe dieser Funktion lässt sich ein beliebiges Multi Dimensionales Array nach einem bestimmten Key sortieren. Funktioniert natürlich nur bei assoziativen Arrays :-)

Die Funktion:

```
unction array_sort($array, $on, $order=SORT_ASC)
{
  $new_array = array();
  $sortable_array = array();

  if (count($array) > 0) {
    foreach ($array as $k => $v) {
      if (is_array($v)) {
        foreach ($v as $k2 => $v2) {
          if ($k2 == $on) {
          $sortable_array[$k] = $v2;
          }
        }
        } else {
          $sortable_array[$k] = $v;
      }
    }

    switch ($order) {
      case SORT_ASC:
      asort($sortable_array);
      break;
      case SORT_DESC:
      arsort($sortable_array);
      break;
    }

    foreach ($sortable_array as $k => $v) {
      $new_array[$k] = $array[$k];
    }
  }

  return $new_array;
}
```

Beispiel:

```
$values[] = array ("id" => "384f5de3ea2fded", sellerid => 1, sellername => "Zeppelin");
$values[] = array ("id" => "384f5de3ea2fdef", sellerid => 4, sellername => "Buxbaum");
$values[] = array ("id" => "384f5de3ea2fde1", sellerid => 3, sellername => "Rosenkranz");
$values[] = array ("id" => "384f5de3ea2fde2", sellerid => 7, sellername => "Lotus");

print_r(array_sort($values, "sellerid"));
print_r(array_sort($values, "sellername"))
```
