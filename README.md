
### HTSlib-ftidx

This version of htslib has a patch to support random access to my experimental file format fastt - ONT fast5 files flattened to tsv.

#### Example usage

##### Building an Index
```c
#include "htslib/ftidx.h"

char *fasttfile = "file.fastt";

// build ftidx
int ret = fti_build(fasttfile);
if (ret != 0) {
    //error handle
}
```

##### Query a given readID using ftidx

```c
#include "htslib/ftidx.h"

char *fasttfile = "file.fastt";
char *readid = "readid";

//load the ftidx
ftidx_t *ftidx= fti_load(fasttfile);
if (ftidx == NULL) {
  //error handle
}

//query a readID
int len=0;
char *record = fti_fetch(ftidx, readid, &len);
if(record==NULL || len <0){
  //query not found or error
}
else{
  printf("%s\n",record);
  free(record);
}

//destroy the index
fti_destroy(ftidx);
```

### Building HTSlib-ftidx

See [INSTALL](INSTALL) for complete details.

```sh
autoreconf     # If using configure, generate the header template...
./configure --enable-bz2=no --enable-lzma=no --with-libdeflate=no --enable-libcurl=no  --enable-gcs=no --enable-s3=no     # Optional, needed for choosing optional functionality
make
make install
```

### HTSlib

Original HTSlib repository : https://github.com/samtools/htslib

HTSlib is an implementation of a unified C library for accessing common file
formats, such as [SAM, CRAM and VCF][1], used for high-throughput sequencing
data, and is the core library used by [samtools][2] and [bcftools][3].
HTSlib only depends on [zlib][4].
It is known to be compatible with gcc, g++ and clang.

HTSlib implements a generalized BAM index, with file extension `.csi`
(coordinate-sorted index). The HTSlib file reader first looks for the new index
and then for the old if the new index is absent.

This project also includes the popular tabix indexer, which indexes both `.tbi`
and `.csi` formats, and the bgzip compression utility.

[1]: http://samtools.github.io/hts-specs/
[2]: http://github.com/samtools/samtools
[3]: http://samtools.github.io/bcftools/
[4]: http://zlib.net/
