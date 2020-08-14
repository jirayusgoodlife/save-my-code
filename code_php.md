Table of Content
================
- [Code get next value number only](#code-get-next-value-number-only)
- [Code get next value a to z only](#code-get-next-value-a-to-z-only)
- [Code get next value A to Z uppercase only](#code-get-next-value-a-to-z-uppercase-only)
- [Code get next value thai language only](#code-get-next-value-thai-language-only)


## Code get next value number only
```php  
function get_value($value,$length_limit=0){
    $value = trim($value);
    if($value == ""){
        return "";
    }
    if(!preg_match("/[0-9]/",$value)){
        return "";
    }
    if($length_limit == 0) {
        $length_limit = strlen($value) + 1;
    }
    return substr(++$value,-$length_limit);
}
```

## Code get next value a to z only
```php  
function get_value($value,$length_limit=0){
    $value = trim($value);
    if($value == ""){
        return "";
    }
    if(!preg_match("/[a-z]/",$value)){
        return "";
    }
    if($length_limit == 0) {
        $length_limit = strlen($value) + 1;
    }
    return substr(++$value,-$length_limit);
}
```

## Code get next value A to Z uppercase only
```php  
function get_value($value,$length_limit=0){
    $value = trim($value);
    if($value == ""){
        return "";
    }
    if(!preg_match("/[A-Z]/",$value)){
        return "";
    }
    if($length_limit == 0) {
        $length_limit = strlen($value) + 1;
    }
    return substr(++$value,-$length_limit);
}
```

## Code get next value thai language only
```php  
function get_value($value, $length_limit = 0)
{
    $value = trim($value);
    if ($value == "") {
        return "";
    }
    if (!preg_match("/[ก-ฮ]/", $value)) {
        return "";
    }
    if ($length_limit == 0) {
        $length_limit = mb_strlen($value, 'utf-8') + 1;
    }
    $array_thai = array(
        'ก', 'ข', 'ค', 'ง', 'จ', 'ฉ', 'ช', 'ซ', 'ณ', 'ญ',
        'ฐ', 'ฑ', 'ฒ', 'ณ', 'ด', 'ต', 'ถ', 'ท', 'ธ', 'น',
        'บ', 'ป', 'ผ', 'ฝ', 'พ', 'ฟ', 'ภ', 'ม', 'ย', 'ร',
        'ล', 'ว', 'ศ', 'ษ', 'ส', 'ห', 'ฬ', 'อ', 'ฮ'
    );

    $size_array_thai = count($array_thai);
    $size_value = mb_strlen($value, 'utf-8');

    $is_last_ch_in_array = false;
    $next_value = "";
    for ($index = 1; $index <= $size_value; $index++) {
        $value_last_str = mb_substr($value, -$index, 1, 'utf-8');
        $pos_last_str = array_search($value_last_str, $array_thai);

        if ($index == 1 || $is_last_ch_in_array) {
            $next_value = $array_thai[($pos_last_str + 1) % $size_array_thai] . $next_value;
            $is_last_ch_in_array = false;
        } else {
            $next_value = $value_last_str . $next_value;
        }
        if ($pos_last_str == $size_array_thai - 1) {
            $is_last_ch_in_array = true;
        }
    }

    if ($is_last_ch_in_array) {
        $next_value = $array_thai[0] . $next_value;
    }

    return mb_substr($next_value, -$length_limit, null, 'utf-8');
}
```

## Code generate ramdom string

``` php
function get_value($length = 64, $is_use_number = true, $is_use_lowercase = true, $is_use_uppercase = true, $is_use_thai_str = false)  
{  
  if ($length < 1) {  
  return "";  
  }  
  $number = "0123456789";  
  $lowercase = "abcdefghijklmnopqrstuvwxyz";  
  $uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";  
  $thai_str = "กขคงจฉชซณญฐฑฒณดตถทธนบปผฝพฟภมยรลวศษสหฬอฮ";  
  $characters = '';  
 if ($is_use_number) {  
  $characters .= $number;  
  }  
  if ($is_use_lowercase) {  
  $characters .= $lowercase;  
  }  
  if ($is_use_uppercase) {  
  $characters .= $uppercase;  
  }  
  if ($is_use_thai_str) {  
  $characters .= $thai_str;  
  }  
  
  $charactersLength = mb_strlen($characters, 'utf-8');  
  $randomString = '';  
 for ($i = 0; $i < $length; $i++) {  
  $randomString .= mb_substr($characters, rand(0, $charactersLength - 1), 1, 'utf-8');  
  }  
  return $randomString;  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0OTYxNDcxMjQsODEyNzM0Mjg3XX0=
-->