Redis Caching Duzenleme
#######################

:date: 2014-12-21
:comments: true 
:categories: redis
:tags: server





**simple write operation:**

1. The client sends a write command to the database (data is in client's memory).
2. The database receives the write (data is in server's memory).
3. The database calls the system call that writes the data on disk (data is in the kernel's buffer).
4. The operating system transfers the write buffer to the disk controller (data is in the disk cache).
5. The disk controller actually writes the data into a physical media (a magnetic disk, a Nand chip, ...).



