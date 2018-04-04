# jit
jit test


x86 jit compile test

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/mman.h>

int exec( unsigned char *code, int size ) {

  void *mem = mmap(NULL, size, PROT_WRITE | PROT_EXEC, MAP_ANON | MAP_PRIVATE, -1, 0);

  memcpy(mem, code, size);

  int (*func)() = mem;

  return func();
}

int do_div( int *a, int *b ) {
  /*
  mov eax, 10
  mov edx, 0
  mov ebx, 2
  div ebx
  ret
  */

   //x86 div
  unsigned char code[] = {
    0xB8, *a, 0x00, 0x00, 0x00, 
    0xBA, 0x00, 0x00, 0x00, 0x00, 
    0xBB, *b, 0x00, 0x00, 0x00, 
    0xF7, 0xF3, 
    0xC3
  };

  return exec( code, sizeof( code ) );
}

int do_mul( int *a, int *b ) {
  /*
  mov eax, 10
mov ebx, 2
mul ebx
ret
*/


   //x86 mul
  unsigned char code[] = {
    0xB8, *a, 0x00, 0x00, 0x00, 
    0xBB, *b, 0x00, 0x00, 0x00, 
    0xF7, 0xE3, 
    0xC3
  };


  return exec( code, sizeof( code ) );
}

int do_sum( int *a, int *b ) {

  //   mov eax, 0
  //   mov ebx, 0
  //   add eax, ebx
  //   ret

  //x86 sum
  unsigned char code[] = {
    0xB8, *a, 0x00, 0x00, 0x00, 
    0xBB, *b, 0x00, 0x00, 0x00, 
    0x01, 0xD8, 
    0xC3 
  };

  return exec( code, sizeof( code ) );
}

int do_sub( int *a, int *b ) {

  /*
mov eax, 10
mov ebx, 2
sub eax, ebx
ret
*/

   //x86 sub
  unsigned char code[] = {
    0xB8, *a, 0x00, 0x00, 0x00, 
    0xBB, *b, 0x00, 0x00, 0x00, 
    0x29, 0xD8,
    0xC3
  };

  return exec( code, sizeof( code ) );
}


int main(int argc, char *argv[]) {
  // Machine code for:


  if (argc < 3) {
    fprintf(stderr, "Usage: ./a.out <integer> <integer>\n");
    return 1;
  }


  int a = atoi(argv[1]);
  int b = atoi(argv[2]);

  printf("%d\n", do_sum( &a, &b ) );
  printf("%d\n", do_sub( &a, &b ) );
  printf("%d\n", do_mul( &a, &b ) );
  printf("%d\n", do_div(  &a, &b ) );

}


```
