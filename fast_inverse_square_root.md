To remark the gorgeous and well known algorithm, 1/(x)^(1/2).

``` C
float Q_rsqrt(float num)
{
    long i;
    float x2, y;
    const float threehalfs = 1.5F;

    x2 = num * 0.5;
    y = num;
    i = *(*long) &y;                      // evil floating point bit level hack
    i = 0x5f3759df - (i >> 1);            // what the fuck?
    y = *(*float) &i;
    y = y * (threehalfs - (x2 * y * y));  // 1st iteration
//  y = y * (threehalfs - (x2 * y * y));  // 2nd iteration, this can be removed

    return y;
}
```

