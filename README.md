# chstr
By nature, Go strings are immutable: a change causes copy. This package provides a tiny type that allows to append and overwrite strings without copy operation.

#### Example

The core method is `Write(runeIndex int, value string) int` that

    package main

    import "github.com/crackgoland/chstr"

    func main() {
      ms := chstr.New("Hello world", 100)
      
      // Here is copy-less operations block. 
      // NOTE: a string to be written is copied, but not the source string.
      ms.Write(0, "Damn")
      ms.Write(0, "Brave")
      ms.Write(14, "小洞不补")
      ms.Write(40, " --- --")
      ms.Write(70, "Привет мир!")
      
      fmt.Println(ms.Get())
    }
    
Prints:

    Brave world小洞不补 --- --Привет мир!
    
NOTE: The output is 33 characters (33 runes). Offsets like 40 and 70 is to demonstrate that if you specify rune index past the end of current length, Write just appends string ignoring that index

Miscellaneous TODO
- [ ] `Append(string) int` method
- [ ] `Load(*string)` method
