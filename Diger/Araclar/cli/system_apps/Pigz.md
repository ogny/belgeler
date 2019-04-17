A parallel implementation of gzip for modern multi-processor, multi-core machines

* sikistirma ve acma :
```
tar cf - paths-to-archive | pigz > archive.tar.gz
pigz -dc target.tar.gz | tar xf -
```
