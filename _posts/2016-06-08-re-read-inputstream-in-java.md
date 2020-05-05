---
title: "Re read inputstream in java"
description: ""
category: 
tags: [java]
---
{% include JB/setup %}

Sometimes in java you get an inputstream, you need to process it the first time to process the data then you need it again to output the contents. To do this you need to read the contents to an byte array then use the ByteArrayInputstream to read the data then use the reset method to return the pointer to the beginning to read again.

{% highlight java %} 
File sourceFile = new File("/tmp/sourcefile.xml");
InputStream inputStream = new FileInputStream(sourceFile);
ByteArrayOutputStream baos = new ByteArrayOutputStream();
org.apache.commons.io.IOUtils.copy(inputStream, baos);
byte[] bytes = baos.toByteArray();

ByteArrayInputStream bais = new ByteArrayInputStream(bytes);

File targetFile = new File("/tmp/targetfile.xml");
FileUtils.copyInputStreamToFile(bais, targetFile);
bais.reset();
processXml(bais);
{% endhighlight %} 

This uses the apache commons io package.
