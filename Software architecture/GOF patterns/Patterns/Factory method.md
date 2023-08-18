![[Pasted image 20230725085931.png]]
![[Pasted image 20230725090006.png]]

```C#
enum WriterType {
	Text, Hex
}

interface IFileWriter {
	void Write(string text);
}

class TextWriter: IFileWriter {
///
}

class HexWriter: IFileWriter {
///
}

static class FactoryWriter {
	IFileWriter GetWriter(int type) {
		return type switch {
			case 0 => new TextWriter();
			case 1 => new HexWriter();
		}
	}
}

var text = "some text";
var writer = FactoryWriter.GetWriter((int)WriterType.Text)
writer.Write(text);
```
