# File size
To get file size, just combine fseek and fseek.

```C
fseek(fd, 0L, SEEK\_END);
size\_t sz = ftell(fd);
fseek(fd, 0L, SEEK\_SET);
```

// there are also some api provide by OS.
