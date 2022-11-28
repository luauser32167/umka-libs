# ufmt - sprintf like formatting library

## Examples
```Go
import (
  std = "std.um";
  uu = "ufmt.um";
)

fn writef(fmt: str, args: ..any) { printf("%s", uu.sfmt(fmt, args)); }
fn writefln(fmt: str, args: ..any) { printf("%s\n", uu.sfmt(fmt, args)); }

fn simple() {
  writefln("a '%%' sign");
  writefln("booleans: %, %", false, true);
  writefln("integers base 10: %, %, %, %", int8(1), int16(10), int32(100), int(-1000));
  writefln("integers base 16: %X, %x, %016X, %X", uint8(0x7F), uint16(0x7FFF), uint32(0x7FFFFFFF), uint(-1));
  writefln("integers base 8: %o, %o", 69, 255);
  writefln("integers base 2: %b, %08b", 10, 21);
  writefln("reals: %1.2, %", 355.0/113.0, 355.0/113.0);
  writefln("strings: '%', '%5', '%-5', %.2", "umka", "umka", "umka", "umka");
}

type FilePrinter = struct {
  f: std.File;
}
fn (x: ^FilePrinter) printBuf(nbuf: int, buf: []char): bool {
   // return nbuf == rtlfwrite(&buf[0], nbuf, sizeof(char), x.f);
   var ch: [1]char;
   for a := 0; a < nbuf; a++ {
    ch[0] = buf[a];
    if 1 != std.fwrite(x.f, ch) {
      return false;
    }
   }
   return true;
}
fn formatToFile() {
  var x: FilePrinter;
  var fname: str = "output.txt";
  x.f = std.fopen(fname, "wb");
  if null != x.f {
    printf("Note: writing to file '%s'\n", fname);
    uu.ufmt(x, "There are % leaves here.\n", 69105);
  }
  std.fclose(x.f);
}

fn countChars() {
  writefln("the string '69105 leaves' has % characters", uu.calcFmtLen("% leaves", 69105));
}

fn main() {
  simple();
  // formatToFile();
  countChars();
}

```
