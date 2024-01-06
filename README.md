# (FTE-)QuakeC Hash Table Generator

`QCHashTableGenerator` is a simple utility designed to take in `.CSV` input files and generate an FTEQCC-compilable hash table structure, hashed using XMODEM CRC16. The overall goal is to ease compute time by mitigating the overall amount of string comparisons your QuakeC project may perform.

It makes the assumption that the first value in your input `.CSV` is the value you want to hash. Given this example, `old_path` will be replaced with a generated hash:

```csv
old_path,current_path
progs/player.mdl,models/player.mdl
```

Producing output similar to this:

```c
var struct {
    float old_path_crc;
    string current_path;
    float crc_strlen;
} struct_name[] = {
    {63799, "models/player.mdl", 16} // progs/player.mdl
}
```

The output is sorted in ascending order, by the hashed strings, so it will be easy to use in a binary search.

The purpose of the `crc_strlen` field is for the develoepr to, if they deem necessary, perform a `strlen` check on their input string and compare it with the provided `crc_strlen` in an effort to detect hash collisions.

## Installation & Usage

`QCHashTableGenerator` is designed to support python 3.7 and above. The following modules are required to run the generator:

```c
colorama==0.4.6
fastcrc==0.3.0
pandas==2.1.4
```

They can be installed automatically with `pip -r requirements.txt`.

`qc_hash_generator.py --help` will give you a list of arguments to use to generate your QuakeC source file. But to just get off the ground, `qc_hash_generator.py -i some.csv` is all you will need.