# retry

A little optimisation of @igorw 's retry

```php
<?php
use function pag\retry;

// retry an operation up to 5 times
$user = retry(5, function () use ($id) {
    return User::find($id);
});

// here is why you want to start using HHVM
$user = retry(5, () ==> User::find($id));

// this is probably a bad idea
$user = retry(INF, () ==> {
    throw new RuntimeException('never gonna give you up');
});
```

```
line     #* E I O op                           fetch          ext  return  operands
-------------------------------------------------------------------------------------
   8     0  E >   RECV                                             !0      
         1        RECV                                             !1      
  12     2    >   INIT_DYNAMIC_CALL                                        !1
         3        DO_FCALL                                      0  $3      
         4      > RETURN                                                   $3
         5*       JMP                                                      ->7
  13     6  E > > CATCH                                       last         'Exception'
  14     7    >   POST_DEC                                         ~4      !0
         8      > JMPNZ                                                    ~4, ->2
  15     9    >   NEW                                              $5      :11
        10        SEND_VAL_EX                                              ''
        11        SEND_VAL_EX                                              0
        12        SEND_VAR_EX                                              !2
        13        DO_FCALL                                      0          
        14      > THROW                                         0          $5
  17    15*     > RETURN                                                   null
```
