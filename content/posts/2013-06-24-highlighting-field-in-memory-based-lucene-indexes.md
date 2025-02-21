---
title: Highlighting field in memory-based Lucene indexes
author: ogirardot
type: post
date: 2013-06-24T21:57:33+00:00

publicize_twitter_user:
  - ogirardot
categories:
  - Data
  - Java
  - Lucene
tags:
  - highlighting
  - ighlighting
  - indexing
  - lucene

---
<p style="text-align:justify;">
  I'm using more and more Lucene these days, and getting in depth on a few subjects, today i'm going to talk to you about how to handle the new Highlighting features available with Lucene 4.1.
</p>
<!--more-->

<p style="text-align:justify;">
  One of the main achievements with this new version is the creation of the great <a href="http://lucene.apache.org/core/4_1_0/highlighter/org/apache/lucene/search/postingshighlight/PostingsHighlighter.html">PostingsHighlighter</a>. Michael McCandless wrote a great piece about it in his article <a title="A new Lucene Highlighter is born" href="http://blog.mikemccandless.com/2012/12/a-new-lucene-highlighter-is-born.html">A new Lucene highlighter is born</a> and i encourage you to read it if you want to get serious about highlighting using Lucene :).
</p>

<p style="text-align:justify;">
  Now let's say you want to use it on a <a href="http://lucene.apache.org/core/4_1_0/memory/org/apache/lucene/index/memory/MemoryIndex.html">MemoryIndex</a>, considering the MemoryIndex as the best In-Memory index type with more than ~500k queries/s handled and the &#8220;perfect&#8221; <strong>reset() </strong>method, it would be great right ? But it's a nice dream as the MemoryIndex doesn't store anything about the raw data, so... we need a plan B.
</p>

<p style="text-align:justify;">
  The plan B can be to use the old-fashioned, but still useful, <a href="http://lucene.apache.org/core/4_1_0/core/org/apache/lucene/store/RAMDirectory.html">RAMDirectory</a> index that will still behave like a normal &#8220;Directory&#8221;-based index and will give you the ability to store the data you need on the field to match. Here is an example on how to use it :
</p>

[code language=&#8221;java&#8221;]  
final int MAX_DOCS = 10;  
final String FIELD_NAME = "text";  
final Directory index = new RAMDirectory();  
final StandardAnalyzer analyzer = new StandardAnalyzer(Version.LUCENE_41);

IndexWriterConfig writerConfig = new IndexWriterConfig(Version.LUCENE_41, analyzer);  
IndexWriter writer = new IndexWriter(index, writerConfig);  
// create document  
Document document = new Document();  
FieldType type = new FieldType();  
type.setIndexed(true);  
type.setStored(true); // it needs to be stored to be properly highlighted  
type.setTokenized(true);  
type.setIndexOptions(FieldInfo.IndexOptions.DOCS\_AND\_FREQS\_AND\_POSITIONS\_AND\_OFFSETS); // necessary for PostingsHighlighter  
document.add(new Field(FIELD_NAME, "this an example of text that must be highlighted", type));  
// add it to the index  
writer.addDocument(document);  
writer.commit();  
writer.close();

Query query = new QueryParser(Version.LUCENE\_41, FIELD\_NAME, analyzer).parse("example");  
DirectoryReader directoryReader = DirectoryReader.open(index);

IndexSearcher searcher = new IndexSearcher(directoryReader);  
PostingsHighlighter highlighter = new PostingsHighlighter();  
TopDocs topDocs = searcher.search(query, MAX_DOCS);  
String[] strings = highlighter.highlight(FIELD_NAME, query, searcher, topDocs);  
System.out.println(Arrays.toString(strings));  
// expected output : [this an <b>example</b> of text that must be highlighted]  
[/code]

<p style="text-align:justify;">
  I'm honestly considering right now to use <strong>both indexes </strong>querying heavily the MemoryIndex and using the RAMDirectory only when i know there's a match found and i need the highlighting features. Maybe i'm not done digging up around this hole and there's a way to make any highlighter work with the MemoryIndex, but i doubt it, both conceptually and after testing everything i could.
</p>

<p style="text-align:justify;">
  If you think otherwise, and know a way to do so, tell me 🙂
</p>

_Vale_