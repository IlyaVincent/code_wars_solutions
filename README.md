# Решение задач codewars.com
Профиль https://www.codewars.com/users/ThakVincent

1) [JavaScript](#javascript)
2) [PHP](#php)

## JavaScript

#### Задача:
Given an array, find the int that appears an odd number of times.
There will always be only one integer that appears an odd number of times.
#### Решение:
```
function findOdd(a) {
  let counts = {},
      num = 0;
  for (let i = 0; i < a.length; i++) {
    num = a[i];
    counts[num] = counts[num] ? counts[num] + 1 : 1;
  }
  for (let key in counts) {
    if (counts[key] % 2 != 0) num = key;
  }
  
  return +num;
}
```

#### Задача:
Write a program that will calculate the number of trailing zeros in a factorial of a given number.
N! = 1 * 2 * 3 * ... * N
#### Решение:
```
function zeros (n) {
  let zero = 0;
  for (let i = 5; i <= n; i *= 5) {
      if (n >= i) zero += Math.trunc(n / i);
  }
  return zero;
}
```

## PHP

#### Задача:
Given two strings s1 and s2, we want to visualize how different the two strings are. We will only take into account the lowercase letters (a to z). First let us count the frequency of each lowercase letters in s1 and s2.
```
s1 = "A aaaa bb c"
s2 = "& aaa bbb c d"
s1 has 4 'a', 2 'b', 1 'c'
s2 has 3 'a', 3 'b', 1 'c', 1 'd'
```
So the maximum for 'a' in s1 and s2 is 4 from s1; the maximum for 'b' is 3 from s2. In the following we will not consider letters when the maximum of their occurrences is less than or equal to 1.

We can resume the differences between s1 and s2 in the following string: "1:aaaa/2:bbb" where 1 in 1:aaaa stands for string s1 and aaaa because the maximum for a is 4. In the same manner 2:bbb stands for string s2 and bbb because the maximum for b is 3.

The task is to produce a string in which each lowercase letters of s1 or s2 appears as many times as its maximum if this maximum is strictly greater than 1; these letters will be prefixed by the number of the string where they appear with their maximum value and :. If the maximum is in s1 as well as in s2 the prefix is =:.

In the result, substrings (a substring is for example 2:nnnnn or 1:hhh; it contains the prefix) will be in decreasing order of their length and when they have the same length sorted in ascending lexicographic order (letters and digits - more precisely sorted by codepoint); the different groups will be separated by '/'. See examples and "Example Tests".

Hopefully other examples can make this clearer.
```
s1 = "my&friend&Paul has heavy hats! &"
s2 = "my friend John has many many friends &"
mix(s1, s2) --> "2:nnnnn/1:aaaa/1:hhh/2:mmm/2:yyy/2:dd/2:ff/2:ii/2:rr/=:ee/=:ss"

s1 = "mmmmm m nnnnn y&friend&Paul has heavy hats! &"
s2 = "my frie n d Joh n has ma n y ma n y frie n ds n&"
mix(s1, s2) --> "1:mmmmmm/=:nnnnnn/1:aaaa/1:hhh/2:yyy/2:dd/2:ff/2:ii/2:rr/=:ee/=:ss"

s1="Are the kids at home? aaaaa fffff"
s2="Yes they are here! aaaaa fffff"
mix(s1, s2) --> "=:aaaaaa/2:eeeee/=:fffff/1:tt/2:rr/=:hh"
```
#### Решение:
```
function breakdown($array) {
  $array = preg_replace('/[^a-z]/', "", $array);
  foreach (count_chars($array, 1) as $i => $val) {
    if ($val < 2) continue;
    $collect[chr($i)] = $val;
  }
  return $collect;
}

function sortArray($a, $b)  {
  if (strlen($a) != strlen($b)) return strlen($b) - strlen($a);
    elseif ($a[0] != $b[0]) return strnatcmp($a[0], $b[0]);
    elseif ($a[2] != $b[2]) return strnatcmp($a[2], $b[2]);
}

function mix($s1, $s2)  {
  $arr[] = breakdown($s1);
  $arr[] = breakdown($s2);

  for ($i = 0; $i < count($arr); $i++) {
    if (empty($arr[$i])) {
      foreach ($arr[$i == 1 ? 0 : 1] as $key => $value) {
        $sortArr[] = ($i == 1 ? 0 : 1) + 1 . ":" . str_repeat($key, $value);
      }
    }
  }
  
  if (!empty($arr[0]) && !empty($arr[1])) {
    foreach ($arr[0] as $key => $value) {
      if ($value == $arr[1][$key]) {
        $sortArr[] = "=:" . str_repeat($key, $value);
      } elseif (isset($arr[1][$key])) {
        if ($value > $arr[1][$key]) {
          $sortArr[] = "1:" . str_repeat($key, $value);
        } else {
          $sortArr[] = "2:" . str_repeat($key, $arr[1][$key]);
        }
      }
    }
    
    $diff[1] = array_diff_key($arr[0], $arr[1]);
    $diff[2] = array_diff_key($arr[1], $arr[0]);
    
    for ($i = 1; $i <= count($diff); $i++) {
      foreach ($diff[$i] as $key => $value) {
        $sortArr[] = $i . ":" . str_repeat($key, $value);
      }
    }
  }

  usort($sortArr, 'sortArray');
  return implode("/", $sortArr);
}
```

#### Задача:
Build Tower
Build Tower by the following given argument:
number of floors (integer and always greater than 0).
Tower block is represented as *
for example, a tower of 3 floors looks like below
```
[
  '  *  ', 
  ' *** ', 
  '*****'
]
```
and a tower of 6 floors looks like below
```
[
  '     *     ', 
  '    ***    ', 
  '   *****   ', 
  '  *******  ', 
  ' ********* ', 
  '***********'
]
```
#### Решение:
```
function tower_builder(int $n): array {
  $start = "*";
  $numStar = -1;
  for ($i = 0; $i < $n; $i++) {
    $numStar += 2;
    $input[$i] = str_pad($start, $numStar, "*", STR_PAD_BOTH);
    $input[$i] = str_pad($input[$i], $n + ($n - 1), " ", STR_PAD_BOTH);
  }
return $input;
}
```

#### Задача:
You are given an array (which will have a length of at least 3, but could be very large) containing integers. The array is either entirely comprised of odd integers or entirely comprised of even integers except for a single integer N. Write a method that takes the array as an argument and returns this "outlier" N.
Examples
```
[2, 4, 0, 100, 4, 11, 2602, 36]
Should return: 11 (the only odd number)

[160, 3, 1719, 19, 11, 13, -21]
Should return: 160 (the only even number)
```
#### Решение:
```
function find($integers) {
  foreach ($integers as $num) {
    ($num % 2) == 0 ? $even[] = $num : $odd[] = $num;
  }
return count($even) > count($odd) ? $odd[0] : $even[0];
}
```

#### Задача:
Write a function that takes an integer as input, and returns the number of bits that are equal to one in the binary representation of that number. You can guarantee that input is non-negative.

Example: The binary representation of 1234 is 10011010010, so the function should return 5 in this case
#### Решение:
```
function countBits($n) {
return substr_count(base_convert($n, 10, 2), '1');
}
```

#### Задача:
Implement the function unique_in_order which takes as argument a sequence and returns a list of items without any elements with the same value next to each other and preserving the original order of elements.
For example:
```
uniqueInOrder('AAAABBBCCDAABBB') == ['A', 'B', 'C', 'D', 'A', 'B']
uniqueInOrder('ABBCcAD')         == ['A', 'B', 'C', 'c', 'A', 'D']
uniqueInOrder([1,2,2,3,3])       == [1,2,3]
```
#### Решение:
```
function uniqueInOrder($iterable) {
	if (empty($iterable)) return [];
	if (is_array($iterable)) $iterable = implode("", $iterable);
  	for ($i = 0; $i < strlen($iterable); $i++) {
   		$current = $iterable{$i};
    	if ($current != $item) {
      	$ret[] = $current;
      	$item = $current;
    	}
  	}  
	return $ret;
}
```

#### Задача:
Write Number in Expanded Form
You will be given a number and you will need to return it as a string in Expanded Form. For example:
```
expandedForm(12); // Should return '10 + 2'
expandedForm(42); // Should return '40 + 2'
expandedForm(70304); // Should return '70000 + 300 + 4'
```
NOTE: All numbers will be whole numbers greater than 0.
#### Решение:
```
function expanded_form(int $n): string {
	$count = strlen($n);
	$sum = (string) $n;
    for($i = 0; $i < $count; $i++) {
   		if($sum{$i} == 0 ) continue;
    	$return .= intval($sum{$i}*pow(10,$count - $i - 1)) . " + ";
    }
  $return = rtrim($return, " +");
  return $return;
}
```
