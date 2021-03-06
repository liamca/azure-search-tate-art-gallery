# Exploring the Tate Gallery Art Collection using Azure Search

This sample shows how [Azure Search](https://azure.microsoft.com/en-us/services/search/
) can be used to explore the [Tate Gallery Art Collection](http://www.tate.org.uk/art/) through full text search.  The metadata along with thumbnail images for their 65,000+ art collection has been generously provided from the following [GitHub Repository](https://github.com/tategallery/collection).  Please look at this page for more information on the license details of this dataset.

<a href="https://rawgit.com/liamca/azure-search-tate-art-gallery/master/Demo/index.html"><img src="https://raw.githubusercontent.com/liamca/azure-search-tate-art-gallery/master/demo_screenshot.png"></a>

## Indexing the Art Collection

The goal of this section will be to upload the metadata for the Art Collection to an Azure Search index.  Once indexed, we can they execute full text searches to retrieve and filter the results based on various categories (facets).  If you have not already created an Azure Search service, you can do so using the [Free Azure Trial](https://azure.microsoft.com/en-us/pricing/free-trial/).  You can also use a Free Azure Search service, but since the free service is limited to 10,000 documents, you will not be able to index the entire collection.  If you want to index the full collection, you might want to choose a [Basic Search Service](https://azure.microsoft.com/en-us/blog/azure-search-introduces-new-lower-cost-tier-in-preview/).  For more details on how to create an Azure Search service, please visit [this page](https://azure.microsoft.com/en-us/documentation/articles/search-create-service-portal/).

In the event, you simply want to play with this index rather than creating your own Azure Search server, I have pre-created an Azure Search index and have included a read-only key that allows you to peform searches.  Please see the section "Searching the Tate Art Collection" for more details on how to do this.<br>
<b>Search Service</b>: azs-playground<br>
<b>Query API Key</b>: 82C2E107EEB733CB613EBD24977BCB0E<br>

Indexing the content is pretty simple after you have created the search service.  Using the included Visual Studio solution, simply apply your Azure Search service name and Azure Search API Key to the Program.cs file and launch the application. The resulting index will have the following schema:

<pre>
<code>
Fields = new[] 
{ 
	new Field("acno",                       DataType.String)         { IsKey = true,  IsSearchable = false, IsFilterable = false, IsSortable = false, IsFacetable = false, IsRetrievable = true},
	new Field("acquisitionYear",            DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("all_artists",                DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("catalogueGroupCompleteStatus",DataType.String)        { IsKey = false, IsSearchable = true,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("catalogueGroupFinbergNumber",DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("catalogueGroupGroupType",    DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("catalogueGroupId",           DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("catalogueGroupShortTitle",   DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("classification",             DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("contributorCount",           DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("contributors",               DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
	new Field("creditLine",                 DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("dateRange",                  DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("dateText",                   DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("depth",                      DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("dimensions",                 DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("foreignTitle",               DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("groupTitle",                 DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("height",                     DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("width",                      DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("id",                         DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("inscription",                DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("medium",                     DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("movementCount",              DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("subjectCount",               DataType.Int32)          { IsKey = false, IsSearchable = false, IsFilterable = true,  IsSortable = true,  IsFacetable = true,    IsRetrievable = true},
	new Field("subjects",                   DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
	new Field("thumbnailCopyright",         DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("thumbnailUrl",               DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true},
	new Field("title",                      DataType.String)         { IsKey = false, IsSearchable = true,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true, Analyzer = AnalyzerName.EnMicrosoft},
	new Field("units",                      DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = true,  IsSortable = true,  IsFacetable = true,  IsRetrievable = true},
	new Field("url",                        DataType.String)         { IsKey = false, IsSearchable = false,  IsFilterable = false,  IsSortable = false,  IsFacetable = false,  IsRetrievable = true}
}
</code>
</pre>

## Searching the Tate Art Collection

After indexing the Tate Art Collection, you can start searching this collection.  To do this, you will need your Azure Search service name and API Key.  Alternatively, you can use the Azure Search playground service (Search Service: azs-playground, Query API Key: 82C2E107EEB733CB613EBD24977BCB0E) to try some queries if you did not create your own search service.  Here is an example:

[https://azs-playground.search.windows.net/indexes/tate-art-collection/docs?$skip=0&$top=10&$select=acno,title,all_artists,thumbnailUrl,url,subjects,dateText,groupTitle,medium&api-version=2015-02-28&$count=true&facet=contributors&api-key=82C2E107EEB733CB613EBD24977BCB0E&facet=subjects&search=nature](https://azs-playground.search.windows.net/indexes/tate-art-collection/docs?$skip=0&$top=10&$select=acno,title,all_artists,thumbnailUrl,url,subjects,dateText,groupTitle,medium&api-version=2015-02-28&$count=true&facet=contributors&api-key=82C2E107EEB733CB613EBD24977BCB0E&facet=subjects&search=nature)

## Online Demo

You can see a simple [demonstration of this here](https://rawgit.com/liamca/azure-search-tate-art-gallery/master/Demo/index.html) along with [associated code](https://github.com/liamca/azure-search-tate-art-gallery/tree/master/Demo).

