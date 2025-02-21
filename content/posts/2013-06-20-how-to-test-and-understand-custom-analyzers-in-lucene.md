---
title: How to test and understand custom analyzers in Lucene
author: ogirardot
type: post
date: 2013-06-20T20:41:05+00:00

publicize_twitter_user:
  - ogirardot
publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:2397699;i:391;}s:2:"wp";a:1:{i:0;i:1;}}'
categories:
  - BigData
  - Data
  - Java
  - Lucene
  - OSS
tags:
  - solr
  - test framework

---
<p style="text-align:justify;">
  I've began to work more and more with the great &#8220;low-level&#8221; library <a href="https://lucene.apache.org">Apache Lucene</a> created by Doug Cutting. For those of you that may not know, Lucene is the indexing and searching library used by great entreprise search servers like Apache Solr and <a href="elasticsearch.org">Elasticsearch</a>.
</p>
<!--more-->

<p style="text-align:justify;">
  When you start to index and search data, most of the time you need to create a <em>filtering and cleaning pipeline </em>to transform your raw text data into something more <strong>indexable </strong>and slightly more <strong>standardized</strong>. Such a pipeline may include <strong>lowercasing, transforming to ascii </strong>or even<strong> stemming </strong>(transforming for &#8220;eating => eat&#8221;). Defining such a pipeline is defining an <strong>Analyzer </strong>in Lucene-world, and while it's a very easy process to create a new/custom one, tweaking it to your needs is another thing and needs thorough testing.<b><br /> </b>
</p>

<p style="text-align:justify;">
  Today's article is precisely to help you out regarding how to test your own analyzer or even create a simple test case for Lucene's analyzers, to allow you to better understand what they do and why they do it.
</p>

<p style="text-align:justify;">
  Luckily for us, using the latest version<strong> Apache Lucene 4.1</strong>, we're not left on our own and we can rely on a few tools because Lucene comes with a test framework that needs a few trick to work, so here we go :
</p>

<p style="text-align:justify;">
  You need testing right, so we need to add the dependency <b>org.apache.lucene:lucene-test-framework </b>as a maven artifact,  but not so fast, the test-framework needs to be before <strong>lucene-core </strong>even if they are in completely different scope, and you need to use at least maven 2.x because otherwise the classpath order won't respect the dependency definition order (what a beautiful world...) :
</p>

[code language=&#8221;xml&#8221;]  
<!- must be before lucene core for classpath issues ->  
<dependency>  
<groupId>org.apache.lucene</groupId>  
<artifactId>lucene-test-framework</artifactId>  
<version>${lucene.version}</version>  
<scope>test</scope>  
</dependency>  
<dependency>  
<groupId>org.apache.lucene</groupId>  
<artifactId>lucene-core</artifactId>  
<version>${lucene.version}</version>  
</dependency>  
[/code]

<p style="text-align:justify;">
  But now if you want to create a new JUnit test for testing the behaviour of an analyzer, you've got access to a new Base Class that you can extends called <strong>BaseTokenStreamTestCase</strong>. But the joy of it all is not exactly to be able to write <strong>&#8220;public class MyWonderfulTestCase extends <strong>BaseTokenStreamTestCase&#8221;</strong></strong> and clap your hands, now you have access to a brand new class of assertions (by the way you need to <strong>enable assertions to execute your tests with the -ea parameter as VM</strong> <strong>args</strong>)
</p>

<ul style="text-align:justify;">
  <li>
     <strong>assertTokenStream </strong>: it allows you to specify the field on which you're testing (otherwize &#8220;dummy&#8221; fieldName gets passed onto the analyzer) and check the token stream output;
  </li>
  <li>
    <strong>assertAnalyzesTo </strong>: you don't specify the field on which you're testing, but it has a simpler syntax.
  </li>
</ul>

<p style="text-align:justify;">
  And there is an example of it all in action :
</p>

[code language=&#8221;java&#8221;]  
@Test  
public void shouldNotAlterKeywordAnalyzed() throws IOException {  
Analyzer myKeywordAnalyzer = new KeywordAnalyzer();

assertTokenStreamContents(  
myKeywordAnalyzer.tokenStream("my\_keyword\_field", new StringReader("ISO8859-1 and all that jazz")), new String[] {  
"ISO8859-1 and all that jazz"  
});

assertAnalyzesTo(myKeywordAnalyzer, "ISO8859-1 and all that jazz", new String[] {  
"ISO8859-1 and all that jazz" // a single token output as expected from the KeywordAnalyzer  
});  
}  
[/code]

Hope it will help you out making your search engines more reliable :),

_Vale_