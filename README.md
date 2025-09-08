# Torvald's uemacs with my modifications

My changes: 
 - TAB defaults to 4 and insert spaces instead of real tabs
(random.c) 
 - Makefile changed to CMakeLists.txt for easier compilation on
non-GNU systems (e.g. Alpine Linux) 
 - README changed to README.md

## Building

Configure:

```sh
cmake -Bbuild
```

Build:

```sh
cmake --build build -j
```

Install (requires root):

```sh
cmake --install build
```

*NOTE:* Sadly the original license has the 'non-comercial' clause, making uemacs
non-libre software. It would be cool to rewrite this whole program and license
it with BSD.

# Copyright Notices:

MicroEMACS 3.9 (c) Copyright 1987 Daniel M. Lawrence. Reference Manual Copyright
1987 by Brian Straight and Daniel M. Lawrence. No copyright claimed for
modifications made by Petri H. Kutvonen.

Original statement of copying policy:

MicroEMACS 3.9 can be copied and distributed freely for any non-commercial
purposes. MicroEMACS 3.9 can only be incorporated into commercial software with
the permission of the current author [Daniel M. Lawrence].
