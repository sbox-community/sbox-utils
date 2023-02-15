# Zip Storer

Storing, modifying, extracting and reading Zip files, Support Zip64. This version is fixed and adapted to s&box. For the usage details, check credits.

```csharp
// Extracting file from Zip
{ 
	using ( ZipStorer zip = ZipStorer.Open( FileSystem.Data, "test.zip", System.IO.FileMode.Open ) )
	{
		// Read the central directory collection
		List<ZipStorer.ZipFileEntry> dir = zip.ReadCentralDir();

		foreach ( ZipStorer.ZipFileEntry entry in dir )
		{
			if ( Path.GetFileName( entry.FilenameInZip ) == "textfile.txt" )
			{
				zip.ExtractFile( entry, Path.GetFileName( entry.FilenameInZip ) );
				break;
			}
		}
	}
}

// Reading file inside Zip
{
	using ( ZipStorer zip = ZipStorer.Open( FileSystem.Data, "test.zip", System.IO.FileMode.Open ) )
	{
		MemoryStream memoryStream = new MemoryStream();

		// Read the central directory collection
		List<ZipStorer.ZipFileEntry> dir = zip.ReadCentralDir();

		foreach ( ZipStorer.ZipFileEntry entry in dir )
		{
			if ( Path.GetFileName( entry.FilenameInZip ) == "textfile.txt" )
			{
				_ = zip.ExtractFileAsync( entry, memoryStream ).Result; //async
				break;
			}
		}

		byte[] fileData = memoryStream.GetBuffer();

		Log.Info( Encoding.ASCII.GetString( fileData ) );

	}
}

// Removing file inside of zip
{ 
	ZipStorer zip = ZipStorer.Open( FileSystem.Data, "test.zip", System.IO.FileMode.Open );

	List<string> fileNames = new() { "textfile.txt" };

	List <ZipStorer.ZipFileEntry> removeList = new List<ZipStorer.ZipFileEntry>();

	// Read the central directory collection
	List<ZipStorer.ZipFileEntry> dir = zip.ReadCentralDir();

	foreach ( ZipStorer.ZipFileEntry entry in dir )
	{
		if ( fileNames.Any( x => x.Contains( entry.FilenameInZip ) ) )
		{
			removeList.Add(entry);
		}
	}
	
	ZipStorer.RemoveEntries(ref zip, removeList );

	zip.Close();
}

// Creating Zip and adding file (or directory) with compression
{
	var tempZip = ZipStorer.Create( FileSystem.Data, "test.zip", string.Empty );
	//tempZip.AddDirectory( ZipStorer.Compression.Deflate, "", "test","test comment" );
	tempZip.AddFile( ZipStorer.Compression.Deflate, "textfile.txt", "test/textfile.txt", "for some comments");
	tempZip.Close();
}
```

 # Credits
https://github.com/jaime-olivares/zipstorer
