# sbox-utils
Utils for s&amp;box

# Tga Decoder:
```csharp
var decoder = TgaDecoderTest.TgaDecoder.FromBinary( FileSystem.Data.ReadAllBytes( "path" ).ToArray() );
//or
//var decoder = TgaDecoderTest.TgaDecoder.FromFile(FileSystem.Data, "path" )

byte[] data = new byte[decoder.Width * decoder.Height * 4];
var index = 0;
for ( int y = 0; y < decoder.Height; y++ )
{
	for ( int x = 0; x < decoder.Width; x++ )
	{
		var color = Color.FromRgba( (uint)decoder.GetPixel( x, y ) );
		data[index++] = (byte)(color.g * 255f);
		data[index++] = (byte)(color.b * 255f);
		data[index++] = (byte)(color.a * 255f);
		data[index++] = (byte)((1 - color.r) * 255f);
	}
}

var texture = Texture.Create( decoder.Width, decoder.Height ).WithData( data ).Finish();
```

 # Credits
 https://github.com/wertrain/tga-decoder-cs
