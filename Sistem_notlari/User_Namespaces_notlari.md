One of the overall goals of namespaces is to support the implementation of
containers, a tool for lightweight virtualization (as well as other purposes)
that provides a group of processes with the illusion that they are the only
processes on the system.

One use of mount namespaces is to create environments that are similar to
chroot jails. However, by contrast with the use of the chroot() system call,
mount namespaces are a more secure and flexible tool for this task



[Kaynak:Namespaces in operation](http://lwn.net/)


