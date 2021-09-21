## 

1. read() and write().

    ssize_t	read(int __fd, void \* __buf, size_t __nbyte). // read a specified number of bytes from __fd.
      
    ssize_t write(int __fd, const void * __buf, size_t __nbyte). // write a specified number of bytes into __fd
      
2. fgets() and fputs()

    char \*fgets(char * __restrict, int, FILE *); // reads an entire line at a time
    
    int	fputs(const char * __restrict, FILE * __restrict); // 
4. getc() and putc()

    int getc(FILE \*); // getc() reads one character at a time
    
    int putc(int, FILE \*); // putc() write one character at time
