# nozip

__nozip__ is a simple deflated zip decompressor written in C.

* Amalgamated source code
* Simple api (check `nozip.h`)
* Small size (`~18kb` dependency)

## Usage Example

#### CMake:

```cmake
add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/deps/nozip")
add_executable(app main.c)
target_link_libraries(app nozip::libnozip)
```

#### File read:

```c
#include <stdio.h>
#include <stdlib.h>

#include "nozip.h"

int main(int argc, char **argv)
{
	FILE *fp;
	nozip_t *zip;
	size_t size;
	size_t num_entries;

	if (argc < 2) {
		fprintf(stderr, "usage: %s file\n", argv[0]);
		return 1;
	}

	if (!(fp = fopen(argv[1], "rb"))) {
		perror(argv[1]);
		return 1;
	}

	nozip_t *zip = malloc(nozip_size(fp)); 
	if (!zip || !(num_entries = nozip_read(zip, fp))) {
		perror(argv[1]);
		return 1;
	}

	entries = nozip_entries(zip);
	printf("entries:\n");
	for (size_t i = 0; i < num_entries; ++i) {
		entry = entries + i;
		output = malloc(entry->uncompressed_size);
		if (nozip_uncompress(zip, entry, output, entry->uncompressed_size) != entry->uncompressed_size)
		{
			// failure
			free(output);
			free(zip);
			return 1;
		}
		printf("%s\n", entry->filename);
		// utilize output
		free(output);
	}

	free(zip);

	return 0;
}

```

#### Memory read:

```c
#include <stdio.h>
#include <stdlib.h>

#include "nozip.h" 

extern const unsigned char data[];
extern const unsigned int  data_length;

int main(int argc, const char **argv)
{
	nozip_t *zip;
	nozip_entry_t *entries, entry;
	size_t num_entries;

	zip = malloc(nozip_size_mem((void*)data, data_length)); 
	if (!zip || !(num_entries = nozip_read_mem(zip, (void*)data, data_length))) {
		return 1;
	}
	entries = nozip_entries(zip);
	printf("entries:\n");
	for (size_t i = 0; i < num_entries; ++i) {
		entry = entries + i;
		output = malloc(entry->uncompressed_size);
		if (nozip_uncompress(zip, entry, output, entry->uncompressed_size) != entry->uncompressed_size)
		{
			// failure
			free(output);
			free(zip);
			return 1;
		}
		printf("%s\n", entry->filename);
		// utilize output
		free(output);
	}

	free(zip);

	return 0;
}
```
