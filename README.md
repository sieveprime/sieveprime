# HolyC

```holyc
U0 SieveOfEratosthenes(I64 limit)
{
  U8 *is_prime = MAlloc(limit + 1);
  MemSet(is_prime, 1, limit + 1);
  is_prime[0] = is_prime[1] = 0;
  
  I64 i, j;
  for (i = 2; i * i <= limit; i++) {
    if (is_prime[i]) {
      for (j = i * i; j <= limit; j += i)
        is_prime[j] = 0;
    }
  }
  
  I64 count = 0;
  for (i = 2; i <= limit; i++) {
    if (is_prime[i]) {
      "%d ", i;
      count++;
    }
  }
  
  "\nFound %d primes.\n", count;
  Free(is_prime);
}

U0 Main()
{
  "Upper limit: ";
  I64 limit = GetI64;
  if (limit >= 2)
    SieveOfEratosthenes(limit);
  GetChar;
}

Main;
```

# Limbo
```
implement SieveOfEratosthenes;

include "sys.m";
    sys: Sys;
include "draw.m";
include "math.m";
    math: Math;

SieveOfEratosthenes: module {
    init: fn(ctxt: ref Draw->Context, argv: list of string);
};

init(ctxt: ref Draw->Context, argv: list of string)
{
    sys = load Sys Sys->PATH;
    math = load Math Math->PATH;

    if (math == nil) {
        sys->fprint(sys->fildes(2), "fail: couldn't load Math module\n");
        exit;
    }

    limit := 1000000;
    
    if (tl argv != nil)
        limit = int hd(tl argv);
		
    n := (limit-1)/2;
    isComposite := array[n] of byte;

    for (p := 0; p < n; p++)
        isComposite[p] = byte 0;

    sqrtLimit := int math->sqrt(real limit);
    
    for (i := 3; i <= sqrtLimit; i += 2) {
        idx := (i-3)/2;
        if (isComposite[idx] == byte 0) {
            for (j := i * i; j <= limit; j += 2*i) {
                if (j % 2 == 1) {
                    markIdx := (j-3)/2;
                    if (markIdx < n)
                        isComposite[markIdx] = byte 1;
                }
            }
        }
    }

    count := 1;
    sys->print("2\n");
    
    for (k := 0; k < n; k++) {
        if (isComposite[k] == byte 0) {
            num := 2*k + 3;
            if (num <= limit) {
                sys->print("%d\n", num);
                count++;
            }
        }
    }
    
    sys->print("\nFound %d primes up to %d\n", count, limit);
}
```
