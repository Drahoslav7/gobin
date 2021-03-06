# gobin
Tool for transforming arbitrary binary resource files to golang's byte array

(basicaly simplified version of [jteeuwen/go-bindata](//github.com/jteeuwen/go-bindata))

## Installation
```bash
go get github.com/Drahoslav7/gobin
````


## Usage
``` bash
gobin [ flags ] FILENAME
```


**Flags**
```
  -format value
        type of byte representation
  -package name
        package name (default "main")
  -step number
        number of bytes per line (default 16)
  -var name
        variable name (default "_")
```


### Example
```
gobin -package gui -var font font.tff > font.go
```
**output**
```golang
package gui

var font = []byte{
        0x00, 0x01, 0x00, 0x00, 0x00, 0x0c, 0x00, 0x80, 0x00, 0x03, 0x00, 0x40, 0x47, 0x53, 0x55, 0x42,
        0x02, 0x13, 0x03, 0x8f, 0x00, 0x00, 0x00, 0xcc, 0x00, 0x00, 0x04, 0x40, 0x4f, 0x53, 0x2f, 0x32,
        0x6f, 0xb2, 0x11, 0xfc, 0x00, 0x00, 0x05, 0x0c, 0x00, 0x00, 0x00, 0x60, 0x63, 0x6d, 0x61, 0x70,
        // . . . 
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
}
````


## Formats

Several output formats implemented
  - hexx `0x00, 0x01, 0x6b` (default)
  - hex  `0x0, 0x1, 0x6b`
  - dec  `0, 1, 107`
  - shexx `"\x00\x01\x6b"`
  - shex  `"\x00\x01k"`
  
**Format comparison:**

  `./gobin -format hexx ./gobin.go | tail -n 5 | head -n 4`
```golang
  0x20, 0x7b, 0x0a, 0x09, 0x69, 0x66, 0x20, 0x61, 0x20, 0x3c, 0x20, 0x62, 0x20, 0x7b, 0x0a, 0x09,
  0x09, 0x72, 0x65, 0x74, 0x75, 0x72, 0x6e, 0x20, 0x61, 0x0a, 0x09, 0x7d, 0x20, 0x65, 0x6c, 0x73,
  0x65, 0x20, 0x7b, 0x0a, 0x09, 0x09, 0x72, 0x65, 0x74, 0x75, 0x72, 0x6e, 0x20, 0x62, 0x0a, 0x09,
  0x7d, 0x0a, 0x7d, 0x0a,
```

  `./gobin -format hex ./gobin.go | tail -n 5 | head -n 4`
```golang
  0x20, 0x7b, 0xa, 0x9, 0x69, 0x66, 0x20, 0x61, 0x20, 0x3c, 0x20, 0x62, 0x20, 0x7b, 0xa, 0x9,
  0x9, 0x72, 0x65, 0x74, 0x75, 0x72, 0x6e, 0x20, 0x61, 0xa, 0x9, 0x7d, 0x20, 0x65, 0x6c, 0x73,
  0x65, 0x20, 0x7b, 0xa, 0x9, 0x9, 0x72, 0x65, 0x74, 0x75, 0x72, 0x6e, 0x20, 0x62, 0xa, 0x9,
  0x7d, 0xa, 0x7d, 0xa,
```

  `./gobin -format dec ./gobin.go | tail -n 5 | head -n 4`
```golang
  32, 123, 10, 9, 105, 102, 32, 97, 32, 60, 32, 98, 32, 123, 10, 9,
  9, 114, 101, 116, 117, 114, 110, 32, 97, 10, 9, 125, 32, 101, 108, 115,
  101, 32, 123, 10, 9, 9, 114, 101, 116, 117, 114, 110, 32, 98, 10, 9,
  125, 10, 125, 10,
```

  `./gobin -format shexx ./gobin.go | tail -n 5 | head -n 4`
```
  "\x20\x7b\x0a\x09\x69\x66\x20\x61\x20\x3c\x20\x62\x20\x7b\x0a\x09"+
  "\x09\x72\x65\x74\x75\x72\x6e\x20\x61\x0a\x09\x7d\x20\x65\x6c\x73"+
  "\x65\x20\x7b\x0a\x09\x09\x72\x65\x74\x75\x72\x6e\x20\x62\x0a\x09"+
  "\x7d\x0a\x7d\x0a"+
```

  `./gobin -format shex ./gobin.go | tail -n 5 | head -n 4`
```golang
  " {\n\tif a < b {\n\t"+
  "\treturn a\n\t} els"+
  "e {\n\t\treturn b\n\t"+
  "}\n}\n"+
```



