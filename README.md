# chstr
By nature, Go strings are immutable: a change causes copy. This package provides type that allows to append and overwrite strings without copy operation.

## Example

The core method is `Write(runeIndex int, value string) int`

    package main

    import "github.com/crackgoland/chstr"

    func main() {
      ms := chstr.New("Hello world", 100) // allocate 100 bytes for maximum string size and set initial content
      
      // Here is copy-less operations block. 
      // NOTE: a string to be written is copied, but not the *ms string.
      ms.Write(0, "Damn")
      ms.Write(0, "Brave")
      ms.Write(14, "小洞不补")
      ms.Write(40, " --- --")
      ms.Write(70, "Привет мир!")
      
      fmt.Println(ms.Get())
    }
    
Prints:

    Brave world小洞不补 --- --Привет мир!
    
NOTE: The output is 33 characters (33 runes). Offsets like 40 and 70 are to demonstrate that if you specify rune index past the end of current length, Write just appends string ignoring that index

Writing more bytes than allocated (100) will panic

### Load
`Load()` function takes string pointer and uses trick. Use it carefully as alternative to `Get()`, that copies resulting string in order to return it - Load will use your string (not tested for GC behaviour)

    // ..

	var s string
    
	ms.Load(&s)
    
	fmt.Println(s)
    
Prints:

    Brave world小洞不补 --- --Привет мир!


# References
Based on great article [Go Mutable Strings](https://medium.com/kokster/mutable-strings-in-golang-298d422d01bc) by Kynan Rilee
