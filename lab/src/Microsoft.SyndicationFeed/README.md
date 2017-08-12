# Microsoft.SyndicationFeed
Microsoft.SyndicationFeed provides APIs similar to .NET XMLReader to simplify the reading and writing of RSS 2.0 and Atom syndication feeds. The syndication feed readers and writers were developed with extensiblity and customization in mind to allow consumers to support their own custom feed elements. Syndication Feed operations are on demand, which enables this library to be used on syndication feeds of arbitrary size or stream latency.

### Requirements:
* [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/)

### Supports:
* .NET Standard 1.3

### Building:
* The solution will build in Visual Studio 2017 after cloning.

### Running Tests:
* Open the solution in Visual Studio 2017.
* Build the Tests project.
* Open the Test Explorer and click "Run All" or run each test individually.


# Examples
A folder with examples can be found [here](examples).

### Create an RssReader and Read a Feed ###
```
using (var xmlReader = XmlReader.Create(filePath))
{
    var feedReader = new Rss20FeedReader(xmlReader);

    while(await feedReader.Read())
    {
        switch (feedReader.ElementType)
        {
            // Read category
            case SyndicationElementType.Category:
                ISyndicationCategory category = await feedReader.ReadCategory();
                break;

            // Read Image
            case SyndicationElementType.Image:
                ISyndicationImage image = await feedReader.ReadImage();
                break;

            // Read Item
            case SyndicationElementType.Item:
                ISyndicationItem item = await feedReader.ReadItem();
                break;

            // Read link
            case SyndicationElementType.Link:
                ISyndicationLink link = await feedReader.ReadLink();
                break;

            // Read Person
            case SyndicationElementType.Person:
                ISyndicationPerson person = await feedReader.ReadPerson();
                break;

            // Read content
            default:
                ISyndicationContent content = await feedReader.ReadContent();
                break;
        }
    }
}
```

### Create an RssWriter and Write an Rss Item ###
```
var sw = new StringWriter();
using (XmlWriter xmlWriter = XmlWriter.Create(sw))
{
    var writer = new Rss20FeedWriter(xmlWriter);
      
    // Create item
    var item = new SyndicationItem()
    {
        Title = "Rss Writer Avaliable",
        Description = "The new Rss Writer is now open source!",
        Id = "https://github.com/dotnet/wcf/tree/lab/lab/src/Microsoft.SyndicationFeed/src",
        Published = DateTimeOffset.UtcNow
    };

    item.AddCategory(new SyndicationCategory("Technology"));
    item.AddContributor(new SyndicationPerson(null, "test@mail.com"));

    await writer.Write(item);
    xmlWriter.Flush();
}
```