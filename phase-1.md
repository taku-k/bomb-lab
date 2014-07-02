gdbを使ってdisasしてみていく 

***
$ gdb bomb 
(gdb) disas main  
  
   0x08048a52 <+162>: call   0x80491fc <read_line>  
   0x08048a57 <+167>: add    $0xfffffff4,%esp  
   0x08048a5a <+170>: push   %eax  
   0x08048a5b <+171>: call   0x8048b20 <phase_1>  
***

とあるのでread_lineという関数で文字列を読み取ってそれをスタックに積み  
phase_1という関数を呼び出してることがわかる.  
続いてphase_1をみていく  
***
(gdb) disas phase_1  
Dump of assembler code for function phase_1:  
   0x08048b20 <+0>: push   %ebp  
   0x08048b21 <+1>: mov    %esp,%ebp  
   0x08048b23 <+3>: sub    $0x8,%esp  
   0x08048b26 <+6>: mov    0x8(%ebp),%eax  
   0x08048b29 <+9>: add    $0xfffffff8,%esp  
   0x08048b2c <+12>:  push   $0x80497c0  
   0x08048b31 <+17>:  push   %eax  
   0x08048b32 <+18>:  call   0x8049030 <strings_not_equal>  
   0x08048b37 <+23>:  add    $0x10,%esp  
   0x08048b3a <+26>:  test   %eax,%eax  
   0x08048b3c <+28>:  je     0x8048b43 <phase_1+35>  
   0x08048b3e <+30>:  call   0x80494fc <explode_bomb>  
   0x08048b43 <+35>:  mov    %ebp,%esp  
   0x08048b45 <+37>:  pop    %ebp  
   0x08048b46 <+38>:  ret  
***
string_not_equalという関数を呼び出していることがわかる.  
0x08048b3aで test %eax %eax にて返り値をチェックしていることがわかる.  
ここでstring_not_equalの第一引数は,0x08048b26にてphase_1の第一引数であることがわかる.  
それは自分が入力した文字列のアドレスである.  
第二引数が怪しいのでこのメモリに格納されている文字列を表示する.  
***
(gdb) x/s 0x80497c0  
0x80497c0:   "Public speaking is very easy."  
***
よってphase_1の答えは  
Public speaking is very easy.  