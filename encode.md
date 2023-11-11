## Encode

##### converting
http://www.asciitable.com/
echo "obase = 2 ; 7" | bc   (7 decimal to binary)
echo "ibase = 2 ; 111" | bc   (binary 111 to decimal)

>>> print('\u265E\u265F')
♞♟

urldecode:
echo aaa+bbb+ccc | php -r 'echo urldecode(fgets(STDIN));'  //aaa bbb ccc
echo aaa+bbb+ccc | php -r 'echo urlencode(fgets(STDIN));'  
echo '&lt;' |recode html..ascii