Table of Content
================
- [Code get next value (number only)](#code-get-next-value-(number-only))
- [Code get next value (a-z only)](#code-get-next-value-(a-z-only))
- [Code get next value (A-Z only)](#code-get-next-value-(A-Z-only))
- [Code get next value (ก-ฮ only)](#code-get-next-value-(ก-ฮ-only))

## Code get next value (number only)
```php  
function get_value($value,$length_limit=0){
    $value = trim($value);
    if($value == ""){
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

## Code get next value (a-z only)
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

## Code get next value (A-Z only)
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

## Code get next value (ก-ฮ only)
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

