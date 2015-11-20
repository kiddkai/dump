# tar

> Just tar

```
man tar
```

## Basic usage

### make a tar - not compress

```bash
tar -cf name.tar.gz file1 file2 file3
```

### make a compressed tar - compress to gzip

```bash
tar -czf name.tar.gz file1 file2 file3
```

### make a compressed tar - compress to compress

```bash
tar -cZf name.tar.gz file1 file2 file3
```

### view tarball content

```bash
tar -tvf name.tar.gz | less
```

### append file to a tar, only work for not compressed tar files

```bash
tar -rf name.tar.gz file-to-append...
```

## Composition

## With loop

```bash
tar -czf $(for f in /path/to/files/*; do process_file; echo "${f}"; done)
```

## With find

```bash
find /path/to/pack -type f -exec tar -czf {}
```

## With xargs

```bash
find /path/to/pack -type f | xargs tar -czf
```

## Compose them

```bash
find /path/to/pack -type f | xargs tar -czf $(for .. in ...; do ... done;)
```
