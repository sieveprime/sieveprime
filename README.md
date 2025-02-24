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
